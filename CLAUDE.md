# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev       # dev server at http://localhost:4321/
npm run build     # production build → dist/
npm run preview   # preview production build locally
```

## Stack

- **Astro 6** with the blog starter template
- **Tailwind CSS v4** (via `@tailwindcss/vite` Vite plugin — no `tailwind.config.*` file)
- **TypeScript** (relaxed strictness)
- Content via Astro's built-in Content Collections (MDX + Markdown)

## Architecture

**Content**: Blog posts live in `src/content/blog/` as `.md`/`.mdx` files. The collection schema is defined in `src/content.config.ts`. Slugs are file-name derived; the catch-all route `src/pages/blog/[...slug].astro` renders them using `src/layouts/BlogPost.astro`.

**Layouts & components**: `BaseHead.astro` handles `<head>` including font loading (local Atkinson woff files in `src/assets/fonts/`) and imports `src/styles/global.css`. Global CSS lives there; Tailwind utilities are available via the `@import "tailwindcss"` at the top of that file.

**Site metadata**: Shared constants (site title, description) are in `src/consts.ts`.

**Fonts**: Atkinson Hyperlegible loaded via Astro's `fontProviders.local()` in `astro.config.mjs`, not via Google Fonts.

**RSS**: Auto-generated at `/rss.xml` via `src/pages/rss.xml.js`.

## Current state (as of 2026-04-18)

- Scaffolded and running locally (`npm run dev` confirmed clean)
- Repo live at https://github.com/Bamskies780/blaker-blog (public, `master` branch)
- **Template content is still in place** — `src/content/blog/` has the Astro sample posts, `src/pages/index.astro` / `about.astro` have placeholder copy. These need to be stripped before going live.
- Netlify not yet connected to this repo — a separate coming-soon page is live via Netlify Drop. Next step is to wire up Netlify CI to this GitHub repo and replace the Drop deploy.
- `site:` in `astro.config.mjs` is still set to `https://example.com` — needs updating to `https://blaker.blog`

## Key constraints

- All prose and copy is written by the owner — do not generate blog content, bio text, or homepage copy.
- This folder lives outside OneDrive intentionally — do not move it.
- Do not push to GitHub or touch Netlify deploys without explicit instruction.
