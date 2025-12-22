# Docsify site (Vercel-ready)

## Deploy on Vercel (GitHub)
1. Push this repo to GitHub.
2. In Vercel: **New Project** → import the repo.
3. Framework preset: **Other**
4. Build Command: *(leave empty)*
5. Output Directory: *(leave empty)*
6. Deploy.

Docsify runs client-side and `vercel.json` rewrites all routes to `docs/index.html`.


## Verify on Vercel
- Open `/docs/README.md` and `/docs/_sidebar.md` to ensure static files are served.
- If you still see `Loading...`, check DevTools → Network for 404 on `.md` files.
