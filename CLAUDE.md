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
- `/projects` - projects page (`src/pages/projects.astro`) - `showDate={false}` on byline
- `/contact` - contact form (`src/pages/contact.astro`) - Netlify form, uses `pubDate={new Date()}`
- `bio.astro` still exists in `src/pages/` but is orphaned — removed from nav, no links to it. Can be deleted.

**Layouts & components**: `BaseHead.astro` handles shared `<head>` metadata, imports `src/styles/global.css`, and runs an inline theme-init script before paint. `Header.astro` uses `Home / Blog / Projects / Contact`. `Footer.astro` includes Instagram, LinkedIn, and a "Get in touch" link. Blog post pages rendered by `src/layouts/BlogPost.astro` use the title-first byline (`Title -> Blake McLemore · date`) and no hero image in the post body.

**Theme**: Light/dark mode via `data-theme` attribute on `<html>`. CSS variables for all colors are defined in `:root` and `:root[data-theme='dark']` in `src/styles/global.css`. The init script in `BaseHead.astro` reads `localStorage` first, then system preference, to avoid flash.

**Site metadata**: Shared constants (site title, description) are in `src/consts.ts`.

**Fonts**: `@fontsource/stix-two-text` (400, 500) for body/headings and `@fontsource-variable/geist-mono` for code. Imported at the top of `src/styles/global.css`. The previous local Atkinson font pipeline was removed.

**RSS**: Auto-generated at `/rss.xml` via `src/pages/rss.xml.js`.

## Staging a new blog post

`master` is wired to Netlify and auto-deploys to production. To stage a post (or any other change) before it goes live, use Netlify Deploy Previews:

1. Create a branch — e.g. `post/<slug>`.
2. Add the `.md`/`.mdx` file under `src/content/blog/` and commit on the branch.
3. Push the branch and open a PR against `master`. Netlify auto-builds a Deploy Preview at `deploy-preview-<n>--<site-name>.netlify.app`.
4. Use the Deploy Preview URL to verify the post renders, internal links work, the blog index slot is correct, and RSS picks it up. Backlinks use relative paths, so anything that works on the preview will work on prod.
5. Merge to `master` to publish live.

Optional layers, **not currently implemented** — only wire up if the owner asks:
- `draft: true` frontmatter flag, filtered out at build time, for keeping unfinished work in `master` without publishing.
- `pubDate > now` filtering for scheduled publishing.

## Current state (as of 2026-05-02)

- **Site is LIVE at [blaker.blog](https://blaker.blog)** — domain cutover complete, HTTPS active
- Netlify is Git-connected; pushes to `master` auto-deploy to production
- Site title in header is `blaker.blog` (set in `src/consts.ts`)
- Light/dark toggle is live in the header and persisted
- Structural pages exist for `/bio`, `/projects`, and `/contact`
- Homepage contains owner-written blurb in `src/pages/index.astro`
- First real post lives at `src/content/blog/why-am-i-starting-a-blog-in-2026.md` and uses a custom thumbnail at `src/assets/why-am-i-starting-a-blog-in-2026-thumbnail.svg`
- Astro sample blog posts were removed from `src/content/blog/`
- The "why am I starting a blog" post is pinned as the featured large card on `/blog`, even after future posts are added
- Contact form on `/contact` posts via Netlify Forms. A static `public/__forms.html` mirror ensures form detection at build time. After submission, Netlify redirects to `/thanks` (rendered by `src/pages/thanks.astro`)
- Bio page removed from nav — `bio.astro` still exists in `src/pages/` but is orphaned (no links to it)
- `site:` in `astro.config.mjs` is set to `https://blaker.blog` ✅

## Next steps

- Delete orphaned `src/pages/bio.astro` (no nav link, no content — safe to remove)

## Key constraints

- All prose and copy is written by the owner - do not generate blog content, bio text, or homepage copy.
- This folder lives outside OneDrive intentionally - do not move it.
- Do not push to GitHub or touch Netlify deploys without explicit instruction.
