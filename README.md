# Fearless — deploy to Vercel

This folder is a complete, deploy-ready project:

```
.
├── index.html          ← the whole app (one file)
└── api/
    └── anthropic.js    ← serverless proxy so the AI coach works publicly
```

Everything in the app works as a plain static site **except** the AI coach and the
AI "quick recap." Those call the Anthropic API, which needs a secret key. The
`api/anthropic.js` function holds that key on the server so it is never exposed in
the browser. If you skip the key, the app still works — the AI features just show a
friendly "couldn't reach the coach" fallback.

---

## Option A — Vercel CLI (fastest)

1. Install the CLI and log in (this opens your browser to authenticate):
   ```
   npm i -g vercel
   vercel login
   ```
2. From inside this folder, deploy:
   ```
   vercel
   ```
   Accept the defaults. You'll get a live `*.vercel.app` URL.
3. Add your Anthropic API key so the AI features work, then redeploy to production:
   ```
   vercel env add ANTHROPIC_API_KEY
   vercel --prod
   ```
   Paste your key (from https://console.anthropic.com) when prompted; choose all
   environments.

## Option B — GitHub + Vercel dashboard

1. Put these files in a new GitHub repository.
2. Go to https://vercel.com/new, import the repo, and click **Deploy**.
3. In the project's **Settings → Environment Variables**, add:
   - Name: `ANTHROPIC_API_KEY`
   - Value: your key from https://console.anthropic.com
4. Trigger a redeploy (Deployments → ⋯ → Redeploy) so the key takes effect.

---

## Important notes

- **Cost / abuse:** Once live, anyone who visits the site and uses the coach is
  spending against *your* Anthropic API key. For a small/shared launch that's fine;
  for a truly public launch, add rate limiting, a simple password, or usage caps.
- **Never put the key in `index.html`.** It belongs only in the Vercel environment
  variable. The included function keeps it server-side.
- **Model:** the app requests `claude-sonnet-4-20250514`. Make sure your API key has
  access to it (swap the model name in `index.html` if needed).
- **No build step:** this is static HTML + one function, so there's nothing to build.
