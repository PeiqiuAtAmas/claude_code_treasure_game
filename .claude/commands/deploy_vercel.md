---
description: Build this project and deploy it to Vercel (production), then report the URL
argument-hint: "[preview]  (omit for a production deploy)"
allowed-tools: Bash(npm run build), Bash(npx vercel *), Bash(npx --yes vercel *), Read, Write, Edit
---

Deploy this local project to Vercel and give me the live URL at the end.

Arguments: `$ARGUMENTS` — if it is `preview`, do a preview deploy; otherwise deploy to production.

Follow these steps in order and stop at the first failure, showing me the error output:

1. **Build locally first** — run `npm run build`. If it fails, stop and show the error; do not deploy a broken build.

2. **Make sure Vercel uses the right output folder.** This Vite project builds to `./build` (see `vite.config.ts` `outDir`), but Vercel's Vite preset expects `./dist`. Check the project-root `vercel.json`:
   - If it does not exist, create it with:
     ```json
     {
       "buildCommand": "npm run build",
       "outputDirectory": "build"
     }
     ```
   - If it exists, make sure `outputDirectory` is `"build"` and keep every other field.

3. **Check Vercel login** — run `npx --yes vercel whoami`. If it reports I'm not logged in, stop and tell me to run `! npx vercel login` myself (it's interactive), then re-run `/deploy_vercel`.

4. **Deploy:**
   - Production (default): `npx --yes vercel deploy --prod --yes`
   - Preview (`$ARGUMENTS` is `preview`): `npx --yes vercel deploy --yes`

   `--yes` links the project to Vercel automatically on the first run (creates `.vercel/`, which is already in `.gitignore`).

5. **Report the result** in Traditional Chinese:
   - The deployment URL printed by the CLI (the production alias ending in `.vercel.app` for prod, or the preview URL)
   - Whether it was a production or preview deploy
   - The Vercel dashboard inspect link if the CLI printed one
