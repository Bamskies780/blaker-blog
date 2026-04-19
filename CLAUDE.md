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

**Layouts & components**: `BaseHead.astro` handles shared `<head>` metadata and imports `src/styles/global.css`. `Header.astro` + `Footer.astro` now share the same social destinations (Instagram + LinkedIn). Blog post pages are rendered by `src/layouts/BlogPost.astro` with title-first metadata and no hero image.

**Site metadata**: Shared constants (site title, description) are in `src/consts.ts`.

**Fonts**: Typography is loaded via `@fontsource` imports in `src/styles/global.css` (`@fontsource/stix-two-text` for body/headings and `@fontsource-variable/geist-mono` for code). The previous local Atkinson font pipeline was removed.

**RSS**: Auto-generated at `/rss.xml` via `src/pages/rss.xml.js`.

## Current state (as of 2026-04-18)

- Scaffolded and running locally (`npm run dev` confirmed clean)
- Repo live at https://github.com/Bamskies780/blaker-blog (public, `master` branch)
- Site title in header is `blaker.blog` (set in `src/consts.ts`)
- Global type scale and spacing were tuned toward the Lee Robinson editorial look (STIX Two Text, narrower reading column, softer headings/lists)
- Blog post header now shows `Title -> Blake McLemore · date` (name/date link to home) and no longer renders the large hero image
- Header and footer social links are aligned to Instagram + LinkedIn
- **Template content is still in place** — `src/content/blog/` has the Astro sample posts, `src/pages/index.astro` / `about.astro` have placeholder copy. These need to be stripped before going live.
- Netlify not yet connected to this repo — a separate coming-soon page is live via Netlify Drop. Next step is to wire up Netlify CI to this GitHub repo and replace the Drop deploy.
- `site:` in `astro.config.mjs` is still set to `https://example.com` — needs updating to `https://blaker.blog`

## Key constraints

- All prose and copy is written by the owner — do not generate blog content, bio text, or homepage copy.
- This folder lives outside OneDrive intentionally — do not move it.
- Do not push to GitHub or touch Netlify deploys without explicit instruction.
