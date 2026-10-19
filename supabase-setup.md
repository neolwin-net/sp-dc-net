# Supabase setup — SP–DC–NET PDF Library

## 1. Create project and admin account
1. Create a project at https://supabase.com/dashboard.
2. Copy the Project URL and publishable key (or legacy anon key) from Project Settings → API / Connect.
3. Under Authentication, create your own email/password user. Disable public sign-ups for this library if you do not need them.
4. Under Storage, create a bucket named `public-pdfs` and mark it **Public**. Set a 25 MB file limit if available.

Public bucket access enables downloads; it does not grant uploads. Upload/delete permissions are controlled by Storage RLS policies.

## 2. Restrict writes to your admin UUID
In Authentication → Users, copy your user UID. Replace `YOUR_ADMIN_USER_UUID` below with that UUID, including the single quotes, then run this in SQL Editor:

```sql
create policy "Public can list published PDFs"
on storage.objects for select to anon, authenticated
using (bucket_id = 'public-pdfs');

create policy "Only PDF library admin can upload"
on storage.objects for insert to authenticated
with check (
  bucket_id = 'public-pdfs'
  and auth.uid() = 'YOUR_ADMIN_USER_UUID'::uuid
  and lower(storage.extension(name)) = 'pdf'
);

create policy "Only PDF library admin can delete"
on storage.objects for delete to authenticated
using (
  bucket_id = 'public-pdfs'
  and auth.uid() = 'YOUR_ADMIN_USER_UUID'::uuid
);

create policy "Only PDF library admin can update"
on storage.objects for update to authenticated
using (bucket_id = 'public-pdfs' and auth.uid() = 'YOUR_ADMIN_USER_UUID'::uuid)
with check (
  bucket_id = 'public-pdfs'
  and auth.uid() = 'YOUR_ADMIN_USER_UUID'::uuid
  and lower(storage.extension(name)) = 'pdf'
);
```

If these policy names already exist, remove the duplicates in Storage → Policies or use unique names. Do not create write policies for the `anon` role.

## 3. Add your public frontend credentials
In `pdf-library.md`, replace `YOUR_SUPABASE_PROJECT_URL` and `YOUR_SUPABASE_PUBLISHABLE_OR_ANON_KEY` in the `PDF_LIBRARY_CONFIG` object. These are public browser values. **Never put a service-role/secret key in GitHub Pages or client-side JavaScript.**

After commit, open `https://neolwin-net.github.io/sp-dc-net/pdf-library/`.

## 4. Test permissions
- Signed out: PDFs should list and download, but uploads are unavailable.
- Signed in as your configured admin UID: upload and delete should work.
- A different authenticated user: upload/delete must be denied by Storage RLS.
- Sign out and check that uploaded PDFs remain publicly downloadable.

## Notes
- This is a static GitHub Pages frontend; Supabase Auth and Storage provide the backend.
- The UI enforces a 25 MB PDF limit. Set the bucket-level limit as an additional guard.
- Every file in this public bucket can be downloaded by anyone with its URL. Do not publish confidential or unauthorized documents.
- The list displays up to 1,000 PDFs in the bucket root.
