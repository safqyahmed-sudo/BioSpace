BioSpace — GitHub Pages + Supabase starter package
=================================================

FILES
- index.html: public student website
- admin.html: separate admin studio, protected by Supabase email/password authentication and an app_metadata role check
- config.js: paste your Supabase project URL and anon/publishable key here
- supabase_setup.sql: create the shared content table, RLS policies and realtime publication

IMPORTANT: This package is prepared for deployment, but it is NOT live until you configure your own Supabase project and publish it to GitHub Pages. Do not share your admin password or put a Supabase service_role key in config.js.

SETUP (free-tier path)
1. Create a Supabase project at https://supabase.com/.
2. In Project Settings > API, copy the Project URL and anon/publishable key into config.js. The public anon key is intended for browser use; the RLS policies are what protect writes.
3. Open SQL Editor in Supabase and run the whole supabase_setup.sql script.
4. In Authentication > Users, create the admin user with email/password.
5. In SQL Editor, run this statement separately after replacing the email:
   update auth.users set raw_app_meta_data = coalesce(raw_app_meta_data, '{}'::jsonb) || '{"role":"admin"}'::jsonb where email = 'YOUR_ADMIN_EMAIL';
   Sign out and sign in again after assigning the role.
6. In Authentication > URL Configuration, add your GitHub Pages student URL and the admin.html URL to allowed redirect URLs if your project requires them.
7. Create a GitHub repository, upload index.html, admin.html, config.js and this README (you may include the SQL file too), then enable Settings > Pages > Deploy from a branch > main / root.
8. Student URL will look like https://YOUR-GITHUB-USERNAME.github.io/REPOSITORY/ . Admin URL will be the same base URL plus admin.html.

HOW CLOUD SYNC WORKS
- Admin edits are stored in Supabase and are shared with every visitor/device.
- Student pages fetch the current shared content and subscribe to realtime updates; they refresh when the admin changes content.
- Folder categories cover videos, playlists, YouTube searches, notes, past papers, lessons, simulations, interactive tools, uploaded files, general links and Google searches.
- Subject/resource emojis can be edited in Admin Studio > Home page editor.
- Only an account whose Supabase app_metadata role is exactly "admin" can write the shared state.

LIMITATIONS / SAFETY
- Supabase and GitHub free plans have usage limits and policies that may change.
- The existing prototype stores uploaded small files and thumbnails as data URLs in JSON state. Avoid large files; prefer public hosted links. For a production site, use Supabase Storage and a dedicated resource table.
- The current shared-state architecture syncs content, not student analytics or student accounts. Do not collect student passwords in the site. If adding student accounts, use Supabase Auth and design privacy-aware event logging separately.
- Public student content is readable by anyone with the site URL. Do not place private files or secrets in the shared state.
- config.js must contain only the public anon/publishable key, never a service_role key.
- If admin login shows an admin-role error, re-run the role update query with the exact email and sign in again to refresh the session.

Helpful links:
GitHub Pages: https://pages.github.com/
Supabase: https://supabase.com/
Supabase Docs: https://supabase.com/docs
