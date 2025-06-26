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


<h1>Redirect URLs on sign in (OTP, Oauth):</h1>
After signing up up a user with OTP and trying to redirect them to a specific page.

For example "localhost:3000/dashboard", you are likely to just get redirected to "localhost:3000".

In the hosted version, you have a very nice UI. That is disabled in the self-hosted one.
![image](https://github.com/user-attachments/assets/29531721-75b2-4f2e-ab4a-15105e7057ea)

To configure in config.toml:
site_url = "env(FRONTEND_DASHBOARD_URL)"
# A list of *exact* URLs that auth providers are permitted to redirect to post authentication.
additional_redirect_urls = ["env(FRONTEND_URL)"]

Where in .env you have:
FRONTEND_URL="http://localhost:3000"
FRONTEND_DASHBOARD_URL="http://localhost:3000/dashboard"

Doing just:
site_url = "env(FRONTEND_URL)/dashboard" will resolve to just "/dashboard", which is not nice.

# Dashboard GraphQL

With the self-hosted [docker setup](https://github.com/supabase/supabase/tree/master/docker), [this graphiql dashboard page](http://localhost:8000/project/default/integrations/graphiql/graphiql) may not have its [GraphQL IDE+docs](https://supabase.com/docs/guides/graphql#supabase-studio) features working with your tables, even though the GraphQL API is working just fine outside of the dashboard. This is due to the Supabases' postgres 'service_role' used by the dashboard not having correct permissions to tables. To fix this:

```
docker exec -it supabase-db /bin/bash
psql --user supabase_admin
postgres=> GRANT SELECT ON ALL TABLES IN SCHEMA public TO service_role;
postgres=> NOTIFY pgrst, 'reload schema';
```

The graphiql IDE should now work properly

<details>
<summary>If you'd like, you can verify permissions to see if you actually need to run the GRANT statements</summary>

Check a schema, in this case "public"
```
SELECT rolname, has_schema_privilege(rolname, 'public', 'USAGE') AS has_usage FROM pg_roles;
```
Should show "service_role | t" for public which is good, but for a schema you created, it may not and so you will need to grant usage permission.

Check one of the tables that doesn't have the GraphQL docs in the dashboard, in this case "mytable"
```
SELECT rolname, has_table_privilege(rolname, 'public.mytable', 'SELECT') AS has_select FROM pg_roles;
```
If you get "service_role | f", then that is causing the issue, and you should run the GRANT statement.
</details>


