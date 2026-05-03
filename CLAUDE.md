# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

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

**Content**: Blog posts live in `src/content/blog/` as `.md`/`.mdx` files. The collection schema is defined in `src/content.config.ts`. Slugs are file-name derived; the catch-all route `src/pages/blog/[...slug].astro` renders them using `src/layouts/BlogPost.astro`.

The blog index in `src/pages/blog/index.astro` sorts posts by `pubDate`, but the post with slug `why-am-i-starting-a-blog-in-2026` is intentionally pinned as the large featured card at the top of `/blog` until the owner decides otherwise. Any later posts should appear below it as the smaller cards.

**Pages**: Current routes:
- `/` - home (`src/pages/index.astro`) - owner-written homepage blurb is in place
- `/blog` - post index (`src/pages/blog/index.astro`)
- `/blog/why-am-i-starting-a-blog-in-2026/` - first published post, intentionally pinned on blog index
- `/bio` - about page (`src/pages/bio.astro`) - still placeholder copy
- `/projects` - projects page (`src/pages/projects.astro`) - `showDate={false}` on byline
- `/contact` - contact form (`src/pages/contact.astro`) - Netlify form, uses `pubDate={new Date()}`

**Layouts & components**: `BaseHead.astro` handles shared `<head>` metadata, imports `src/styles/global.css`, and runs an inline theme-init script before paint. `Header.astro` uses `Home / Blog / Projects / Contact`. `Footer.astro` includes Instagram, LinkedIn, and a "Get in touch" link. Blog post pages rendered by `src/layouts/BlogPost.astro` use the title-first byline (`Title -> Blake McLemore · date`) and no hero image in the post body.

**Theme**: Light/dark mode via `data-theme` attribute on `<html>`. CSS variables for all colors are defined in `:root` and `:root[data-theme='dark']` in `src/styles/global.css`. The init script in `BaseHead.astro` reads `localStorage` first, then system preference, to avoid flash.

**Site metadata**: Shared constants (site title, description) are in `src/consts.ts`.

**Fonts**: `@fontsource/stix-two-text` (400, 500) for body/headings and `@fontsource-variable/geist-mono` for code. Imported at the top of `src/styles/global.css`. The previous local Atkinson font pipeline was removed.

**RSS**: Auto-generated at `/rss.xml` via `src/pages/rss.xml.js`.

## Current state (as of 2026-04-26)

- Remote GitHub `master` is up to date with the changes made in this session
- Site title in header is `blaker.blog` (set in `src/consts.ts`)
- Light/dark toggle is live in the header and persisted
- Structural pages exist for `/bio`, `/projects`, and `/contact`
- Homepage now contains owner-written blurb in `src/pages/index.astro`
- First real post now lives at `src/content/blog/why-am-i-starting-a-blog-in-2026.md`
- That first post uses a custom thumbnail asset at `src/assets/why-am-i-starting-a-blog-in-2026-thumbnail.svg`
- Astro sample blog posts were removed from `src/content/blog/`
- The "why am I starting a blog" post is pinned as the featured large card on `/blog`, even after future posts are added
- `/bio` still contains placeholder copy and should be replaced before going live
- Netlify is not yet connected to this repo - a separate coming-soon page is live via Netlify Drop. Next step is to wire up Netlify CI to the GitHub repo and replace the Drop deploy.
- `site:` in `astro.config.mjs` is still `https://example.com` - needs updating to `https://blaker.blog`

## Next steps

- First code change before launch: update `site:` in `astro.config.mjs` from `https://example.com` to `https://blaker.blog`
- Connect the GitHub repo to Netlify as a Git-backed site
- Set Netlify production branch to `master`
- Use `npm run build` as the build command and `dist` as the publish directory
- Add `blaker.blog` as the production domain in Netlify
- Decide DNS approach:
  - If keeping external DNS, Netlify docs prefer `www.blaker.blog` as primary and apex redirecting to it
  - If using Netlify DNS, `blaker.blog` can be the primary domain directly
- Recommended workflow after launch:
  - `master` deploys production
  - feature branches should be reviewed with Netlify Deploy Preview URLs from pull requests
  - merge to `master` when ready to publish live

## Key constraints

- All prose and copy is written by the owner - do not generate blog content, bio text, or homepage copy.
- This folder lives outside OneDrive intentionally - do not move it.
- Do not push to GitHub or touch Netlify deploys without explicit instruction.
