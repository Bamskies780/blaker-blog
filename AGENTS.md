# AGENTS.md

## Commands

```bash
npm run dev       # dev server at http://localhost:4321/
npm run build     # production build -> dist/
npm run preview   # preview production build locally
```

## Stack

- **Astro 6** with the blog starter template
- **Tailwind CSS v4** via `@tailwindcss/vite`
- **TypeScript** with relaxed strictness
- Content collections for Markdown and MDX

## Architecture

- Blog content: `src/content/blog/`
- Content schema: `src/content.config.ts`
- Blog post route: `src/pages/blog/[...slug].astro`
- Blog post layout: `src/layouts/BlogPost.astro`
- Blog index: `src/pages/blog/index.astro`
- Featured post stays pinned at the top of `/blog`: `why-am-i-starting-a-blog-in-2026`
- Shared metadata: `src/consts.ts`
- Global styles and theme vars: `src/styles/global.css`
- Shared head logic: `src/components/BaseHead.astro`
- RSS: `src/pages/rss.xml.js`

## Routes

- `/` -> `src/pages/index.astro`
- `/blog` -> `src/pages/blog/index.astro`
- `/bio` -> `src/pages/bio.astro`
- `/projects` -> `src/pages/projects.astro`
- `/contact` -> `src/pages/contact.astro`

## Current state

- Site title is `blaker.blog`
- Light/dark toggle is live and persisted
- `/bio`, `/projects`, and `/contact` exist
- Sample Astro blog posts were removed
- Future posts in `src/content/blog/` auto-appear on `/blog`
- `src/pages/index.astro` and `/bio` still contain placeholder copy
- Netlify is not yet connected to this repo
- `astro.config.mjs` still uses `https://example.com` for `site:`

## Response style

- Be concise and direct
- Prefer action over explanation
- Keep status updates short
- Do not add filler or unnecessary summaries

## Constraints

- Do not generate blog content, bio text, or homepage copy
- Do not move this folder
- Do not push to GitHub or touch Netlify without explicit instruction
