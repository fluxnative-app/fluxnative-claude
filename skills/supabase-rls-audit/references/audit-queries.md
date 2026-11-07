# Read-only audit queries

Send each query as one `supabase_read` call with `action: "query"` (a read-only transaction).

- Every query is a `select` on the system catalogs, or an aggregate.
- None of them returns a user row, an email or an object name.
- Replace `('public')` with every exposed schema. Schema names are identifiers you choose, not user input.
- These queries ran on a Supabase Postgres 17 stack. The probes depend on the Management API running a multi-statement script as one transaction, so treat that part as unverified on hosted.

## Q0. Exposed schemas (best effort)

```sql
select rolname, rolconfig from pg_roles where rolname = 'authenticator';
```

- If `rolconfig` carries `pgrst.db_schemas=…`, those schemas are exposed.
- Hosted projects often keep this setting outside the role. Then assume `public` plus every schema the app names with `.schema()`, and say that the list was assumed.

## Q1. Tables: RLS and client grants

```sql
select n.nspname as schema, c.relname as name, c.relkind as kind,
  c.relrowsecurity as rls_enabled,
  (select count(*) from pg_policy p where p.polrelid = c.oid) as policies,
  has_table_privilege('anon', c.oid, 'SELECT') as anon_read,
  has_table_privilege('anon', c.oid, 'INSERT, UPDATE, DELETE') as anon_write,
  has_table_privilege('authenticated', c.oid, 'SELECT') as auth_read,
  has_table_privilege('authenticated', c.oid, 'INSERT, UPDATE, DELETE') as auth_write,
  greatest(c.reltuples, 0)::bigint as approx_rows
from pg_class c
join pg_namespace n on n.oid = c.relnamespace
where n.nspname in ('public')
  and c.relkind in ('r', 'p')
  and not exists (select 1 from pg_depend d where d.objid = c.oid and d.deptype = 'e')
order by c.relrowsecurity, n.nspname, c.relname;
```

How to read the result:

| Row | Meaning |
|---|---|
| `rls_enabled = false` with any client grant | `SUPABASE_RLS_DISABLED` |
| `rls_enabled = false` with no client grant | Latent: not reachable today, fix it anyway |
| `rls_enabled = true`, `policies = 0` | Every client read is refused: a broken feature, not a leak |
| `policies > 0`, `rls_enabled = false` | The policies do nothing until RLS is on |

`approx_rows` is the planner's estimate, `0` if the table was never analysed. It only says whether real data is at stake.

## Q2. Policies on exposed tables

```sql
select schemaname as schema, tablename as name, policyname as policy, cmd, roles,
  qual as using_expr, with_check as check_expr,
  (coalesce(qual, '') ~* '^\(?\s*true\s*\)?$' or coalesce(with_check, '') ~* '^\(?\s*true\s*\)?$') as always_true,
  (coalesce(qual, '') || ' ' || coalesce(with_check, '')) ~ 'auth\.(uid|jwt)\(\)' as checks_identity,
  (coalesce(qual, '') || ' ' || coalesce(with_check, '')) ~* 'user_metadata' as reads_user_metadata,
  (coalesce(qual, '') || ' ' || coalesce(with_check, '')) ~ 'auth\.role\(\)' as uses_auth_role
from pg_policies
where schemaname in ('public')
  and permissive = 'PERMISSIVE'
  and roles && array['public', 'anon', 'authenticated']::name[]
order by schemaname, tablename, cmd, policyname;
```

What each part means:

- **`roles = {public}`** means every role, `anon` included. Prefer naming roles with `to`.
- **The two expressions:**
  - `using` filters existing rows, for `select`, `update` and `delete`;
  - `with check` validates new rows, for `insert` and `update`;
  - an `all` or `update` policy without `with check` reuses its `using`.
- **`always_true`** on any command is a finding. It is intentional only on a `select` policy over public data.
- **`checks_identity = false`** on per-user data is a finding, unless the policy calls a helper function that checks identity itself. Read that helper with Q4b.
- **`uses_auth_role`** usually means "any signed-in user". Treat it like `true` on per-user data.

## Q3. Views, materialized views and foreign tables

```sql
select n.nspname as schema, c.relname as name, c.relkind as kind,
  coalesce(array_to_string(c.reloptions, ',') ~* 'security_invoker=(true|on|1|yes)', false) as security_invoker,
  has_table_privilege('anon', c.oid, 'SELECT') as anon_read,
  has_table_privilege('authenticated', c.oid, 'SELECT') as auth_read,
  case when c.relkind in ('v', 'm') then pg_get_viewdef(c.oid) ~ 'auth\.users' else false end as reads_auth_users
from pg_class c
join pg_namespace n on n.oid = c.relnamespace
where n.nspname in ('public') and c.relkind in ('v', 'm', 'f')
  and not exists (select 1 from pg_depend d where d.objid = c.oid and d.deptype = 'e')
order by n.nspname, c.relname;
```

What to flag:

- A view (`v`) with `security_invoker = false` that a client role can read bypasses RLS on the tables under it.
- A materialized view (`m`) or foreign table (`f`) that a client role can read has no RLS at all.
- `reads_auth_users = true` with a client grant exposes account data.

## Q4. `security definer` functions callable by clients

```sql
select n.nspname as schema, p.proname as name,
  pg_get_function_identity_arguments(p.oid) as args,
  has_function_privilege('anon', p.oid, 'EXECUTE') as anon_exec,
  has_function_privilege('authenticated', p.oid, 'EXECUTE') as auth_exec,
  coalesce(array_to_string(p.proconfig, ','), '') ~ 'search_path=' as pinned_search_path
from pg_proc p
join pg_namespace n on n.oid = p.pronamespace
where n.nspname in ('public') and p.prosecdef
  and p.prorettype <> 'trigger'::regtype
  and not exists (select 1 from pg_depend d where d.objid = p.oid and d.deptype = 'e')
order by n.nspname, p.proname;
```

Trigger functions are excluded because a client cannot call them.

### Q4b. One function's definition

Read the definition of a function that Q4 flags, or that Q2's policies call:

```sql
select pg_get_functiondef($1::regprocedure) as definition;
```

`params: ["public.function_name(argtype, argtype)"]`, using the `args` types from Q4.

A client-callable definer function must derive the caller from `(select auth.uid())` and must not trust an id passed in as an argument. `pinned_search_path = false` is a hardening fix in its own right.

## Q5. Buckets

```sql
select b.id, b.name, b.public, b.file_size_limit, b.allowed_mime_types,
  (select count(*) from storage.objects o where o.bucket_id = b.id) as objects
from storage.buckets b
order by b.name;
```

`supabase_read` with `action: "list_buckets"` returns the same `public` flag without SQL. Match each bucket to what the app stores in it (Gather first, step 2).

## Q6. `storage.objects` policies

```sql
select policyname as policy, cmd, roles,
  qual as using_expr, with_check as check_expr,
  (coalesce(qual, '') || ' ' || coalesce(with_check, '')) ~ 'bucket_id' as names_bucket,
  (coalesce(qual, '') || ' ' || coalesce(with_check, '')) ~ 'auth\.(uid|jwt)\(\)' as checks_identity,
  (coalesce(qual, '') || ' ' || coalesce(with_check, '')) ~ 'foldername' as uses_folder
from pg_policies
where schemaname = 'storage' and tablename = 'objects'
order by cmd, policyname;
```

- **Owner-scoped** means the policy names the bucket and checks identity: either the first path segment is the user id, or `owner_id` equals the caller.
- **Unscoped** means `names_bucket = false`, so the policy covers every bucket, or a user-content bucket with `checks_identity = false`.

## P1–P2. Access probes (read-only impersonation)

Run each probe as one `supabase_read` `query` call holding the whole script. Postgres runs a multi-statement string as one transaction, so the `set local` changes end with the call.

- Add one `union all` line per table and bucket to check.
- If the call refuses multiple statements, or returns no rows, skip the probes and report that they did not run.

P1, as `anon` (signed out):

```sql
set transaction read only;
set local role anon;
select 'public.notes' as target, count(*) as visible_rows from public.notes
union all select 'storage:uploads', count(*) from storage.objects where bucket_id = 'uploads';
```

P2, as a signed-in stranger who owns nothing:

```sql
set transaction read only;
set local role authenticated;
select set_config('request.jwt.claims', json_build_object('sub', gen_random_uuid(), 'role', 'authenticated')::text, true) is not null as impersonating;
select 'public.notes' as target, count(*) as visible_rows from public.notes
union all select 'storage:uploads', count(*) from storage.objects where bucket_id = 'uploads';
```

What to expect:

| Target | Expected `visible_rows` |
|---|---|
| Per-user table, user-content bucket | 0 for both probes |
| Intentionally public data | Above 0 |
| A non-zero you cannot explain | A leak: report it under Q1 or Q2's code |

- A `permission denied for table` error means no grant. That is a pass for that role.
- Write paths are judged from the `with check` expressions in Q2. Do not probe writes: a write probe is a write.
