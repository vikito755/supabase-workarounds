Supabase, the managed version is nice. This is not a critique, just what has worked for me, hoping it helps other people as well.

If this repo has helped you, star it.

The self-hosted version leaves a lot to be desired. This repo is an attempt to document convenient workarounds.

Feel free to submit your via a pull request.

<h1>Configuring authentication:</h1>

In the supabase hosted version we have a convenient UI for the "Authentication" tab like so:
![image](https://github.com/user-attachments/assets/38257b46-b034-47d0-8069-57b92cc9d6c9)

In the self-hosted version we have just this one:
![image](https://github.com/user-attachments/assets/337d438f-52eb-4544-8563-423b4fe5abf6)

A significant downgrade in convenience.

To navigate to the same URLs (provided you use the Supabase CLI, running on - http://127.0.0.1:54323/project/default/)

- Sign In / Up menu - http://127.0.0.1:54323/project/default/auth/providers
- Sessions - http://127.0.0.1:54323/project/default/auth/sessions
- Rate Limits - http://127.0.0.1:54323/project/default/auth/rate-limits
- Emails - http://127.0.0.1:54323/project/default/auth/templates
- Multi factor - https://supabase.com/dashboard/project/default/auth/mfa
- URL configuration - https://supabase.com/dashboard/project/default/auth/url-configuration
- Auth protection - https://supabase.com/dashboard/project/default/auth/protection
- Advanced - https://supabase.com/dashboard/project/default/auth/advanced
