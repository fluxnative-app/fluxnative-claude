# Fix migrations

Write the fix as one new file, `supabase/migrations/<UTC YYYYMMDDHHMMSS>_<what>.sql`.

- Apply it only after the user's explicit yes, one statement per `supabase_sql` call, in file order.
- Put a one-line comment above each block saying who gains or loses access. The user reads them.
- Replace `notes`, `user_id` and `uploads` with the real names.

## Statement order

The order below never locks owners out, and never leaves a table open while the fix is half applied.

1. Create the scoped policies first. On a table with RLS still off they change nothing.
2. Enable RLS. Owners keep working at once, through step 1's policies.
3. Drop the permissive policies. While they existed, the table was as open as before, and no more.
4. Revoke the grants that no client needs, for example all of them from `anon` on per-user tables.
5. Fix views and functions.
6. Fix buckets. A bucket made private needs the app's signed-URL change shipped with it.

`create policy` has no `if not exists`. Write `drop policy if exists "<name>" on <table>;` just before each `create policy`, so a partly applied file can be re-run.

## Owner-scoped table

```sql
-- notes: each signed-in user reads and changes only their own rows; signed-out users get nothing.
drop policy if exists "Owners read notes" on public.notes;
create policy "Owners read notes" on public.notes
  for select to authenticated using ((select auth.uid()) = user_id);
drop policy if exists "Owners add notes" on public.notes;
create policy "Owners add notes" on public.notes
  for insert to authenticated with check ((select auth.uid()) = user_id);
drop policy if exists "Owners edit notes" on public.notes;
create policy "Owners edit notes" on public.notes
  for update to authenticated using ((select auth.uid()) = user_id) with check ((select auth.uid()) = user_id);
drop policy if exists "Owners delete notes" on public.notes;
create policy "Owners delete notes" on public.notes
  for delete to authenticated using ((select auth.uid()) = user_id);
alter table public.notes enable row level security;
drop policy if exists "Enable read access for all users" on public.notes;
revoke all on public.notes from anon;
```

- Writing `(select auth.uid())` rather than `auth.uid()` lets Postgres evaluate it once per statement instead of once per row.
- An index on `user_id` keeps the policy cheap as the table grows.
- If inserts do not send `user_id`, add `alter table public.notes alter column user_id set default auth.uid();`.

## Tables created after Supabase's grant change

Since 2026-05-30 for new projects, and from 2026-10-30 for existing ones, a new `public` table gets no client grants. Grant exactly what the policies allow, next to them:

```sql
grant select, insert, update, delete on public.notes to authenticated;
```

## Public read, owner write

```sql
-- posts: anyone may read published posts; only the author writes.
drop policy if exists "Anyone reads published posts" on public.posts;
create policy "Anyone reads published posts" on public.posts
  for select to anon, authenticated using (published);
-- Author write policies as in the owner-scoped example, with author_id in place of user_id.
```

## A column the owner must not change

RLS is per row. To stop a user raising their own `role` or `plan` on a row they own, remove the client's update right on that column:

```sql
revoke update on public.profiles from authenticated;
grant update (display_name, avatar_path) on public.profiles to authenticated;
```

Set roles and plans from an Edge Function that holds the secret key, or keep them in `app_metadata`.

## Views and functions

```sql
-- profile_cards: the view now obeys the RLS of the tables it reads.
alter view public.profile_cards set (security_invoker = true);

-- grant_credits: clients may no longer call it; the Edge Function that needs it uses the secret key.
revoke execute on function public.grant_credits(uuid, integer) from public, anon, authenticated;

-- every security definer function: pin the search path.
alter function public.grant_credits(uuid, integer) set search_path = '';
```

With `search_path = ''`, the function's body must schema-qualify every name, as in `public.notes` and `auth.uid()`. Read it with Q4b first, and do not pin a body that would then break.

## Buckets and storage policies

```sql
-- uploads: files are no longer readable by URL alone; the app reads them through signed URLs.
update storage.buckets set public = false where id = 'uploads';
```

The owner-scoped `storage.objects` policies follow FluxNative's Supabase Storage recipe (`search_services`, "Supabase Storage owner folder policies"):

- objects live at `{uid}/…`;
- each policy names `bucket_id = 'uploads'`;
- each checks `(storage.foldername(name))[1] = (select auth.uid())::text`.

Write one policy per command:

- **`select`** to read and to create a signed URL;
- **`insert`** to upload;
- **`update` together with `select`** for `upsert`;
- **`delete`** to remove a file.

Drop any policy that has no `bucket_id`, because it covers every bucket.

## Rollback

- A rollback is a new migration that reverses the statements: drop the new policies, re-create the old ones from Q2's output, and restore grants.
- Keep Q2's output for the touched tables in the report, so the old policies can be re-created exactly.
- Never roll back with `disable row level security` on a table that holds user data.
