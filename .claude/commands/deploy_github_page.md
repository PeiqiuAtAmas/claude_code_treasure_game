---
description: Build this project and deploy it to GitHub Pages (gh-pages branch), then report the URL
allowed-tools: Bash(git *), Bash(npm run build*), Bash(npx --yes gh-pages *), Bash(touch build/.nojekyll), Read
---

Deploy this local project to GitHub Pages and give me the live URL at the end.

Follow these steps in order and stop at the first failure, showing me the error output:

1. **Check the git remote** — run `git rev-parse --is-inside-work-tree` and `git remote get-url origin`.
   - If this is not a git repo, or there is no `origin` remote, or `origin` does not point to `github.com`, stop and tell me to create a GitHub repo and connect it myself, e.g.:
     ```
     git init && git add -A && git commit -m "Initial commit"
     git remote add origin https://github.com/<user>/<repo>.git
     git push -u origin main
     ```
     then re-run `/deploy_github_page`.
   - Parse `<owner>` and `<repo>` from the remote URL (works for both `https://github.com/<owner>/<repo>.git` and `git@github.com:<owner>/<repo>.git`).

2. **Build locally with a relative base path** — run `npm run build -- --base=./`.
   GitHub Pages serves the site under `https://<owner>.github.io/<repo>/`, so asset URLs must be relative; passing `--base` on the command line keeps `vite.config.ts` unchanged, so the Vercel deploy (served from `/`) is unaffected. If the build fails, stop and show the error; do not deploy a broken build.

3. **Disable Jekyll** — run `touch build/.nojekyll` so GitHub Pages serves the files as-is.

4. **Publish** — run `npx --yes gh-pages -d build --dotfiles -m "Deploy to GitHub Pages"`.
   This pushes the contents of `./build` (the `outDir` in `vite.config.ts`) to the `gh-pages` branch of `origin`. It uses my existing git credentials; if the push is rejected for authentication, stop and tell me to fix my GitHub credentials, then re-run the command.

5. **Report the result** in Traditional Chinese:
   - The live URL: `https://<owner>.github.io/<repo>/` (if `<repo>` is `<owner>.github.io`, the URL is `https://<owner>.github.io/`)
   - That the first deploy can take 1–2 minutes to go live
   - A reminder that if the page shows 404, I should open the repo's **Settings → Pages** and set **Source** to "Deploy from a branch" with branch `gh-pages` / `(root)` (private repos need a paid GitHub plan for Pages)
