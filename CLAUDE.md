# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a portfolio website for Rachel Weiss built with Astro 5.1.2, a static site generator. The site showcases design and art projects with a focus on performance and clean architecture.

## Development Commands

```bash
npm run dev      # Start development server on localhost:4321
npm run build    # Build for production (outputs to ./dist)
npm run preview  # Preview production build locally
```

## Architecture

### Content Collections
Portfolio projects are managed through Astro's content collections in `/src/content/work/`. Each project is a Markdown file with frontmatter validated by the schema in `src/content.config.ts`:

```typescript
{
  title: string
  description: string
  publishDate: Date
  tags: string[]
  img: string          // Path to image in /public/assets/
  img_alt?: string
  pagesDir?: string    // Directory of page images (e.g., /assets/work/abcsbook)
  pageCount?: number   // Number of pages in the directory
}
```

Multi-page projects store pre-rendered page images in `/public/assets/work/[project]/page-N.jpg`. The `PageGallery` component renders these as a thumbnail grid with GLightbox for fullscreen viewing.

### Page Structure
- **File-based routing**: Pages in `/src/pages/` map directly to routes
- **Dynamic routes**: Work pages use `[...slug].astro` for individual project pages
- **Layout inheritance**: All pages extend `BaseLayout.astro` which handles common elements

### Component Architecture
Components in `/src/components/` are self-contained Astro components that:
- Use scoped styles within `<style>` tags
- Accept props with TypeScript interfaces
- Handle both light/dark theme variants through CSS custom properties

### Styling System
- Global styles in `/src/styles/global.css` define CSS custom properties
- Theme colors use semantic naming (e.g., `--accent-purple-light`, `--gray-999`)
- Responsive breakpoints: 50rem (tablet), 20rem (mobile)
- Dark/light theme toggled via `data-theme` attribute on `<html>`

## Deployment

The site deploys to Fly.io via Docker:
- Production URL: https://rachelweiss.me
- Static assets served by Nginx
- Build artifacts from `./dist` directory
- Auto-scaling configured in `fly.toml`

## Key Files

- `astro.config.mjs` - Astro configuration (site URL, integrations)
- `src/content.config.ts` - Content collection schemas
- `src/components/Icon.astro` - Icon system using SVG sprites
- `src/components/ThemeToggle.astro` - Dark/light mode switcher