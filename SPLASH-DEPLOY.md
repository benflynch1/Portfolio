# Splash page — deployment notes

The splash page lives in `/splash` and is a self-contained copy: hero + client marquee only,
with its own fonts and portrait images. Nothing in it links back to the full site.

The full site (`index.html` and all case studies) is untouched. Nothing about how you work day
to day changes — keep editing files in the root of this repo as normal.

---

## The setup

Two Vercel projects, one repo, no branches.

| Project | Serves | Reachable by |
|---|---|---|
| **portfolio-splash** (new) | `/splash` folder | Public — your domain points here |
| **portfolio** (existing) | repo root, full site | Only you, once protection is on |

---

## One-time setup

### 1. Push the splash folder

```
git add splash SPLASH-DEPLOY.md
git commit -m "Add splash page"
git push
```

### 2. Create the new Vercel project

- Vercel dashboard → **Add New → Project** → import `benflynch1/Portfolio` again
- Name it `portfolio-splash`
- Expand **Root Directory** → set it to `splash`
- Framework Preset: **Other**
- Deploy

You'll get a `.vercel.app` URL. Check the splash renders correctly before going further.

### 3. Lock down the full site

In the **existing** portfolio project:

- **Settings → Domains** → remove `benjaminlynch.co.uk` (and the `www` variant)
- **Settings → Deployment Protection** → enable **Vercel Authentication**, and set the scope to
  **All Deployments** (not Standard Protection — that leaves production public)
- Save

The full site is now visible only when you're signed in to Vercel. Anyone else gets a login wall.

### 4. Point the domain at the splash

In `portfolio-splash` → **Settings → Domains** → add `benjaminlynch.co.uk` and `www.benjaminlynch.co.uk`.

If the domain is already in your Vercel account the DNS is fine as-is — the move takes effect in
seconds. If you're bringing it from an external registrar, follow Vercel's DNS instructions on
that screen.

---

## Day to day

- Keep working on the full site in the repo root exactly as before.
- Every push deploys **both** projects. The splash only changes if you edit `/splash`.
- Preview the full site at its `.vercel.app` URL while signed in to Vercel.

## Launch day

1. `portfolio-splash` → Settings → Domains → remove both domains
2. `portfolio` (full site) → Settings → Domains → add both domains back
3. `portfolio` → Settings → Deployment Protection → turn Vercel Authentication off
4. Optional: delete the `portfolio-splash` project and the `/splash` folder

---

## Worth checking

**Is the GitHub repo private?** If `benflynch1/Portfolio` is public, the full site's source is
readable on GitHub regardless of Vercel protection. Repo → Settings → General → Danger Zone →
Change visibility.
