---
name: supabase-rls-audit
description: "Use before shipping a FluxNative app on a connected Supabase project, or when the user shares a SUPABASE_* finding from FluxNative's Security check: a read-only audit of RLS, grants, views, functions and storage buckets with supabase_sql, then owner-scoped fixes applied only with the user's consent."
---

# Audit and fix Supabase RLS and storage access

## When to use

- The user shares a finding from FluxNative's Security check: `SUPABASE_RLS_DISABLED`, `SUPABASE_POLICY_PERMISSIVE`, `SUPABASE_BUCKET_PUBLIC` or `SUPABASE_STORAGE_POLICY_UNSCOPED`.
- Before the user starts a store build of an app on a connected Supabase project, after a new table or bucket, or when the user asks whether their data is safe.
- A query or upload fails with `new row violates row-level security policy` after hardening.

Every Supabase tool takes the app's id as `project` (find it with `list_apps`, the `app-workflow` skill), and works only while the app has a Supabase connection; on an app without one it is refused, and the user connects Supabase with the project's Server button in the FluxNative builder.

Why this matters:

- Every bundle ships the project URL and the anon or publishable key, by design. Anyone can call the Data API with them.
- Grants and Row Level Security are the only boundary. Missing RLS in AI-built Supabase apps is a published vulnerability class (CVE-2025-48757, disputed by the vendor).

Facts to keep straight:

- `supabase_read` (`action: "query"`) and `supabase_sql` run as owner through the Management API and **bypass RLS**. What they return is not what the app sees. Their results enter this conversation, so query catalog metadata and aggregates only.
- `supabase_read` `query` runs in a read-only transaction and refuses a write: use it for every audit query and probe. `supabase_sql` changes the live database, which no FluxNative revision undoes; use it only to apply a migration the user approved.
- **Exposure needs a grant and then passes RLS.** A role reaches a table only with a privilege (`grant select … to anon`), and RLS then filters rows.
  - Older projects granted every new `public` table to `anon` and `authenticated` automatically.
  - Supabase stopped that for new projects on 2026-05-30 and does so for existing projects on 2026-10-30. Existing tables keep their grants. Read both.
- **`authenticated` is anyone who signed up.** With open sign-up, `to authenticated using (true)` is public. Anonymous sign-ins use the `authenticated` role too.
- **Permissive policies are OR-ed.** One `using (true)` opens the table, whatever the other policies say.
- **Views run as their owner** unless `security_invoker = true` (Postgres 15+), so a view over a protected table skips its RLS. Materialized views and foreign tables cannot have RLS at all.
- **`security definer` functions** in an exposed schema can be called through `rpc()` by any role holding EXECUTE (PUBLIC by default), and they run as their owner.
- **Buckets.**
  - A public bucket serves every object to anyone who has its URL, with no auth.
  - Uploads, updates and deletes still need `storage.objects` policies in every bucket.
  - A `select` policy on a public bucket only adds listing of object names.
- **`user_metadata` (`raw_user_meta_data`) is user-editable.** Never authorize on it. Use `app_metadata` or a table.

## Gather first

1. `connection_info`: a Supabase connection exists and its `managementApi` metadata is true. `supabase_read` `query` and `supabase_sql` need the connection's personal access token.
   - If the token is missing, ask the user to add it in the FluxNative builder with the project's Server button (Server › Backend › Supabase). Never ask for it in chat.
   - Meanwhile do a source-only review and report the database as unchecked.
2. Source: `grep` with `include` set to `src/services/**` for `.from(`, `.rpc(`, `.schema(`, `.storage.from(`, `getPublicUrl`, `createSignedUrl` and `upload(`. From the hits, list every table, function and bucket the app touches, and how (read or write, public URL or signed URL).
3. `list_files` with `glob` set to `supabase/migrations/*.sql`, then `read_file` each one for the intended schema, owner columns (`user_id`, `owner_id` → `auth.users.id`) and policies.
4. Exposed schemas:
   - `public` is exposed by default.
   - Any schema the app reaches with `.schema('x')` is exposed too.
   - Q0 in the reference may show the configured list. Otherwise say which list you assumed.

## Procedure

1. **Audit.** Run Q1–Q6 from [references/audit-queries.md](references/audit-queries.md), one per `supabase_read` call with `action: "query"`. Each is a `select` on catalogs or an aggregate. Change only the schema list and names.
2. **Classify** each object, quoting the evidence (object, role, command, expression):
   - **`SUPABASE_RLS_DISABLED`**: a table in an exposed schema with `rls_enabled = false` that `anon` or `authenticated` can read or write.
     - The same table with no client grant is **latent**, one grant away from open. Fix it anyway.
   - **`SUPABASE_POLICY_PERMISSIVE`**, any of:
     - a permissive policy for `anon`, `authenticated` or `public` with `always_true`;
     - `auth.role() = 'authenticated'` or `auth.uid() is not null` on per-user data;
     - a write policy without an ownership predicate;
     - a policy that reads `user_metadata`.
     - Report a view without `security_invoker`, or a `security definer` function, that a client role can use here too, naming the object kind. The effect is the same: rows reach a role that no policy limits.
     - `checks_identity = false` on a policy that calls a helper function is not a finding yet. Read the helper with Q4b first.
   - **`SUPABASE_BUCKET_PUBLIC`**: `public = true` on a bucket the app writes user content into, such as uploads under a user id, documents, recordings or private photos.
     - Deliberately public assets, such as catalog images or marketing media, are not a finding. Record them as intentional; the user can mark the finding intentional in the Security check.
   - **`SUPABASE_STORAGE_POLICY_UNSCOPED`**: a `storage.objects` policy for a client role that has either:
     - no `bucket_id = '…'` (it then covers every bucket); or
     - no owner predicate on a user-content bucket.
     - A broad `select` policy on a public bucket, which lets clients list every file, also counts.
3. **Decide intent** per object before writing any SQL (see Decisions). Public-read data is legitimate: the fix is anon read plus owner write, not owner-only.
4. **Write the fix as source** in a new `supabase/migrations/<UTC YYYYMMDDHHMMSS>_<what>.sql`. Never edit a migration that was already applied.
   - Use the templates and the statement order in [references/fix-migrations.md](references/fix-migrations.md):
     1. create the scoped policies;
     2. enable RLS;
     3. drop the permissive policies;
     4. revoke the grants nothing needs;
     5. fix views and functions;
     6. fix buckets.
   - That order neither locks owners out nor leaves a window open.
   - For object paths and the storage owner predicate, follow FluxNative's Supabase Storage recipe: `search_services` with a query such as "Supabase Storage owner folder policies".
5. **Change the app code the fix affects, in the same change.** Commit the new migration and these edits together with `apply_changes`, each edited path with the `expected_sha256` from your last read:
   - A bucket made private breaks `getPublicUrl`. Read through `createSignedUrl` in the storage service.
   - Inserts must set the owner column (or rely on `default auth.uid()`) so the new `with check` passes.
   - Features that read other users' rows, such as feeds or shared lists, need a policy for that relationship, never `true`.
6. **Ask before applying.** Ask the user in chat, with each statement in plain words: who loses access, what changes in the app, and how to roll back.
   - Warn the user that app versions already in the stores use the same database and do not get the new code. A bucket made private breaks their image URLs.
7. **Apply only after the user's explicit yes to this migration**, given after you showed it. Run the migration one statement per `supabase_sql` call, in file order.
   - Stop at the first error and report which statements ran.
   - Never improvise a different statement to get past an error.
8. Re-run Q1–Q6 and the probes in Verify and report.

## Decisions

- **Public catalog table.**
  - Use: RLS on; `for select to anon, authenticated using (true)`; no client write policy (or an admin-only write).
  - Report it as intentional. A read-only `true` policy is not a write hole.
- **Shared data** (family, team, chat).
  - Write a membership predicate: `exists (select 1 from members m where m.group_id = t.group_id and m.user_id = (select auth.uid()))`.
  - The membership table's own policies are owner-scoped.
  - If the policies recurse, use a `security definer` helper in a non-exposed schema (`private`) with `set search_path = ''`.
- **RLS on with no policies.** Nothing leaks, but the app's queries come back empty or refused. That is a broken feature, not a finding.
- **Work that must cross users** (admin, counters, webhooks, entitlement grants): an Edge Function holding the secret key, shipped with `supabase_functions deploy` (`search_services` "Supabase Edge Functions" for the pattern). Never widen a policy for it.
- **A view that bypasses RLS.** Use `alter view … set (security_invoker = true)`. If it must stay definer, revoke `select` from `anon` and `authenticated`, or move it to a non-exposed schema.
  - A view that reads `auth.users` leaks emails. Remove its client grants.
- **A `security definer` function.** Decide whether clients should call it at all.
  - If not: `revoke execute … from public, anon, authenticated`.
  - If yes: it must check `(select auth.uid())` itself, and pin `search_path = ''`.
- **A user-content public bucket.** Make it private and serve files through signed URLs. Public avatars may stay public. Then scope writes to the owner's folder and drop broad `select` policies.
- **Declined, or no answer.** Apply nothing. Keep the migration in source as the proposed fix and report the findings as open.
- **No SQL** (no access token, or the user only asked a question or for a plan). Review the migrations in source and report the live database as unchecked.

## Never

- Never run `alter`, `create`, `drop`, `grant`, `revoke`, `update`, `insert` or `delete` without the user's explicit yes to that statement, given after you showed it, even "just enabling RLS". A yes to an earlier fix does not carry over.
- Never select user rows, emails or object names from app tables, `auth.users` or `storage.objects`. Only `count(*)` and catalog metadata. Do not use `supabase_read` `list_objects` or `list_users` for the audit either.
- Never use `disable row level security`, `using (true)` on a write, `grant all … to anon`, or `user_metadata` checks to make a feature work.
- Never put the service-role or secret key in the app, or in `.env`, to get past RLS. Privileged work belongs in an Edge Function, whose secrets the user adds under Server › API keys & secrets and `supabase_functions push_secrets` delivers by name.
- Never treat a `query` result as proof of what the app can read. It bypasses RLS; only the probes impersonate.
- Never edit an applied migration, or delete data, buckets or objects (`delete_bucket`, `delete_objects`) as part of a fix.
- Never call the database secure. Report what was checked.

## Verify and report

- **Re-run Q1–Q6.** Every finding is gone or recorded as intentional with its reason.
- **Probes** (P1–P2 in the audit reference), run as `anon` and as a fresh random `authenticated` user:
  - per-user tables and user-content buckets count 0;
  - only intentional public data counts above 0;
  - if the tool refuses a multi-statement script, say the probes did not run.
- **Two accounts.** User B must not read user A's rows or files. Ask the user to test it: A creates an item, and B opens A's item by its link or id in the preview or on a device. Never create accounts or type passwords without a go-ahead.
- **Report:**
  - each finding: object, code, evidence, the fix, and whether it was applied;
  - the migration path;
  - what was checked: schemas, number of tables, policies and buckets.
- **Also report what was not checked:**
  - other schemas;
  - the bodies of functions you did not flag;
  - Edge Functions that use the secret key;
  - Realtime authorization (`realtime.messages`);
  - Auth settings (open sign-up, anonymous sign-ins);
  - older app versions in the stores.
