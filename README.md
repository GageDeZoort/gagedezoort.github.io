# Personal site (Astro)

Modern rebuild of `gagedezoort.github.io` powered by [Astro](https://astro.build). The repository currently serves a design-forward academic landing page while keeping the legacy Jekyll content archived under `archive/` for reference.

## Local development

```bash
npm install
npm run dev
```

Visit `http://localhost:4321` to preview changes. Edit files in `src/` – the dev server hot-reloads automatically.

## Structure

- `src/layouts/Layout.astro` – global chrome, typography, and metadata helpers.
- `src/pages/index.astro` – homepage content blocks (hero, focus areas, highlights, contact).
- `public/` – static assets served as-is (`/images`, `/docs` for the CV download, etc.).
- `archive/` – frozen copy of the previous Jekyll site.

## Deployment

GitHub Actions build and publish the site to GitHub Pages on every push to `main` (`.github/workflows/deploy.yml`). The workflow uses Node 20 as recommended by Astro. If you push from a feature branch, trigger a manual deploy via the workflow dispatch UI after merging.

For local production builds:

```bash
npm run build
npm run preview
```

This exports the static site to `dist/` and serves it for inspection.
