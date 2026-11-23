# Physioultant (فیزیوشاور) v1.7.0
https://physioultant.configym.com/ · Developer: [Iman Kiani](https://imankiani.com/) · Educational only, not medical advice · EN / FA / FR

## Layout
- `public/` – the finished website (served by Cloudflare Pages)
- `functions/api/analyze.js` – server-side function that talks to Google Gemini (holds NO secrets)
- `source/` – editable sources + build tool (not served)
- No `package.json` at the root, so Cloudflare runs no build.

## Deploy on Cloudflare Pages
1. Upload this whole folder to a new GitHub repo (keep `public`, `functions`, `source`; use git/GitHub Desktop or drag the folders).
2. Cloudflare → Workers & Pages → Create → Pages → Connect to Git → pick the repo.
3. Framework preset **None** · Build command **(empty)** · Build output directory **`public`** · Deploy.
4. **Add the API key as a secret (never in GitHub):** Pages project → Settings → Variables and Secrets → Production → Add → Type **Secret**, name `GEMINI_API_KEY`, value = your NEW Google AI Studio key → Save → **Retry deployment**.
   Optional variables: `GEMINI_MODEL` (default `gemini-3.5-flash`; check the current model name in AI Studio), `ALLOWED_ORIGIN` (default `https://physioultant.configym.com`; comma-separate extra origins such as your `*.pages.dev` preview).
5. Custom domains → add `physioultant.configym.com` (remove an old DNS record that points to GitHub first).
6. Security: Cloudflare → Security → WAF → Rate limiting rule for URI path `/api/analyze` (e.g. 10 requests / 10 min / IP). In Google Cloud restrict the key to the Gemini API and set a budget alert.
7. Test: open `/`, `/fa`, `/fr`, press "Describe your problem by voice" (Chrome/Edge/Safari; needs HTTPS).

## Update the site later
```
cd source && npm install
CLEAN=1 SITE_URL=https://physioultant.configym.com node build.js
```
Copy the contents of `source/dist/` into `public/` (keep `_headers` and `_redirects`), commit, push.
Functions changes are edited directly in `functions/`.

See `source/SECURITY.md` (data flow, residual risks) and `CREDITS.md` (image licence check).
