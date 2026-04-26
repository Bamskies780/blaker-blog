# AGENTS.md

This file provides guidance to Codex (Codex.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev       # dev server at http://localhost:4321/
npm run build     # production build -> dist/
npm run preview   # preview production build locally
```

## Stack

- **Astro 6** with the blog starter template
- **Tailwind CSS v4** (via `@tailwindcss/vite` Vite plugin - no `tailwind.config.*` file)
- **TypeScript** (relaxed strictness)
- Content via Astro's built-in Content Collections (MDX + Markdown)

## Architecture

**Content**: Blog posts live in `src/content/blog/` as `.md`/`.mdx` files. The collection schema is defined in `src/content.config.ts`. Slugs are file-name derived; the catch-all route `src/pages/blog/[...slug].astro` renders them using `src/layouts/BlogPost.astro`. The blog index at `src/pages/blog/index.astro` sorts posts by newest `pubDate` first, but `why-am-i-starting-a-blog-in-2026` is intentionally pinned as the large featured card at the top. Any additional posts render below it as the smaller cards.

**Pages**: Current routes:
- `/` - home (`src/pages/index.astro`) - placeholder copy, needs owner-written content
- `/blog` - post index (`src/pages/blog/index.astro`)
- `/bio` - about page (`src/pages/bio.astro`) - previously `about.astro`, renamed
- `/projects` - projects page (`src/pages/projects.astro`) - `showDate={false}` on byline
- `/contact` - contact form (`src/pages/contact.astro`) - Netlify form, uses `pubDate={new Date()}`

**Layouts & components**: `BaseHead.astro` handles shared `<head>` metadata, imports `src/styles/global.css`, and runs an inline theme-init script before paint. `Header.astro` has an icon-based light/dark toggle (sun/moon + sliding thumb, `aria-pressed`, persisted via `localStorage`). `Footer.astro` social links: Instagram + LinkedIn. Blog post pages rendered by `src/layouts/BlogPost.astro` - title-first byline (`Title -> Blake McLemore · date`), no hero image.

**Theme**: Light/dark mode via `data-theme` attribute on `<html>`. CSS variables for all colors are defined in `:root` and `:root[data-theme='dark']` blocks in `src/styles/global.css`. The init script in `BaseHead.astro` reads `localStorage` then system preference to avoid flash.

**Site metadata**: Shared constants (site title, description) are in `src/consts.ts`.

**Fonts**: `@fontsource/stix-two-text` (400, 500) for body/headings and `@fontsource-variable/geist-mono` for code. Imported at the top of `src/styles/global.css`. The previous local Atkinson font pipeline was removed.

**RSS**: Auto-generated at `/rss.xml` via `src/pages/rss.xml.js`.

## Current state (as of 2026-04-19)

- Running locally (`npm run dev` confirmed clean)
- Repo live at https://github.com/Bamskies780/blaker-blog (public, `master` branch)
- Site title in header is `blaker.blog` (set in `src/consts.ts`)
- Light/dark toggle is live in the header and persisted
- Structural pages added: `/bio`, `/projects`, `/contact` (Netlify form)
- `about.astro` was renamed to `bio.astro`
- Astro sample blog posts have been removed. Future posts added to `src/content/blog/` will automatically appear on `/blog`, and `why-am-i-starting-a-blog-in-2026` should remain the featured top post until the owner decides otherwise.
- `src/pages/index.astro` and `/bio` still have placeholder copy that should be replaced before going live.
- Netlify is not yet connected to this repo - a separate coming-soon page is live via Netlify Drop. Next step is to wire up Netlify CI to the GitHub repo and replace the Drop deploy.
- `site:` in `astro.config.mjs` is still `https://example.com` - needs updating to `https://blaker.blog`

## Key constraints

- All prose and copy is written by the owner - do not generate blog content, bio text, or homepage copy.
- This folder lives outside OneDrive intentionally - do not move it.
- Do not push to GitHub or touch Netlify deploys without explicit instruction.
