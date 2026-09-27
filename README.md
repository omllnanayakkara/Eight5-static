# Eight5.lk — coming soon page

A single static page announcing [Eight5.lk](https://eight5.lk) while the app in
`jobtrackingapp.frontend` is being built. No build step, no framework — plain
HTML and CSS, deployed to Azure Static Web Apps.

## Layout

```
src/                        # the deployed app root (app_location)
  index.html
  styles.css                # theme tokens copied from the app's globals.css
  favicon.svg
  robots.txt
  staticwebapp.config.json  # navigation fallback + security headers
.github/workflows/          # GitHub Actions deploy
```

The colour tokens, Plus Jakarta Sans type and the animated hero grid are taken
from `jobtrackingapp.frontend/app/globals.css`, so this page matches the app.
If the app's theme changes, update the `:root` block in `src/styles.css`.

## Preview locally

Any static server works:

```powershell
npx serve src          # or: python -m http.server 8080 --directory src
```

Or just open `src/index.html` in a browser.

## Deploy

### Option 1 — GitHub Actions (recommended)

1. Push this folder to a GitHub repo.
2. In the Azure portal create a **Static Web App**, choose the repo/branch, and
   set build details to **Custom** with app location `src`, no API, no output
   location. Azure adds the deploy token as a repo secret.
3. If you'd rather keep the workflow in this repo, delete the one Azure commits
   and keep `.github/workflows/azure-static-web-apps.yml`, adding the portal's
   deployment token as the secret `AZURE_STATIC_WEB_APPS_API_TOKEN`.

### Option 2 — Azure CLI / SWA CLI (no repo)

```powershell
az staticwebapp create -n eight5-comingsoon -g <resource-group> -l eastasia2 --sku Free
npm install -g @azure/static-web-apps-cli
swa deploy ./src --env production --deployment-token <token-from-portal>
```

## Custom domain

Add `eight5.lk` under **Custom domains** in the Static Web App, then point DNS
at the hostname Azure gives you (`ALIAS`/`CNAME` for the apex via your DNS
provider, or delegate the zone to Azure DNS). TLS is issued automatically.
