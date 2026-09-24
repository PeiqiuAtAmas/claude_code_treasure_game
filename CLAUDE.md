# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

"Interactive Treasure Box Game" — a single-page React 18 + TypeScript app built with Vite 6 (SWC plugin). The project was exported from Figma Make, which explains the large shadcn/ui component set and the unusual Vite config. It is also used as a Claude Code tutorial project (see `README.md` for the planned exercises: sound effects, win/tie/loss result display, custom key cursor, SQLite sign-in/scores, Vercel/GitHub Pages deploy commands).

## Commands

```bash
npm install
npm run dev     # Vite dev server on http://localhost:3000 (auto-opens browser)
npm run build   # Production build to ./build (not ./dist)
```

There is no test runner, linter, or `tsconfig.json` configured — Vite/SWC strips types without type-checking, so type errors will not fail the build.

## Architecture

- **All game logic lives in `src/App.tsx`.** State is `boxes` (3 boxes, one randomly assigned `hasTreasure`), `score`, and `gameEnded`. Opening a treasure box gives +100, a skeleton box −50. The game ends when the treasure is found or all boxes are opened; "Play Again" calls `initializeGame()`. Note `openBox` calls `setScore` from inside the `setBoxes` updater using the closed-over `score`.
- Animations use `motion/react` (Framer Motion's successor): chest flip (`rotateY`), hover/tap scaling, and fade-in result labels.
- Static assets are imported as ES modules: images in `src/assets/` (closed/opened/skeleton chests, `key.png` for a cursor), sounds in `src/audios/` (`chest_open.mp3`, `chest_open_with_evil_laugh.mp3`). The audio files are already imported in `App.tsx` but not yet played. `src/results/` holds reference screenshots, not app code.
- `src/components/ui/` is the stock shadcn/ui (Radix-based) library; only `Button` is currently used. `cn()` helper is in `src/components/ui/utils.ts`. `src/components/figma/ImageWithFallback.tsx` is a Figma Make helper.
- `@` is aliased to `./src`. The versioned aliases in `vite.config.ts` (e.g. `'sonner@2.0.3': 'sonner'`) exist because Figma-generated code imports packages with version suffixes — keep them if such imports remain.

## Styling caveat

`src/index.css` (imported by `main.tsx`) is a **pre-compiled Tailwind v4 output** — Tailwind is not installed and there is no PostCSS/Tailwind step in the build. Only utility classes already present in `index.css` will work; a new class (e.g. an arbitrary `cursor-[url(...)]`) will silently have no effect. For new styling, either use classes already in the file, inline `style` props, or add plain CSS. `src/styles/globals.css` holds the theme tokens (CSS variables) but is not imported at runtime.

`src/guidelines/Guidelines.md` is an empty Figma Make template with no active rules.
