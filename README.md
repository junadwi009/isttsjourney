# Institut STTS Virtual Campus Tour

Virtual campus tour for **Institut Sains dan Teknologi Terpadu Surabaya**, created by **Arustudio**.

## Repository layout

- `index.html` — public visitor tour; this is what Vercel should serve.
- `studio/index.html` — local no-code Campus Studio editor/CMS.
- `src/` — source modules used to develop the Studio/runtime.
- `tests/` — automated tests and fixtures.
- `tools/` — local utilities.
- `docs/` — QA and preview documentation.
- `ISTTS-Template.campus.json` — initial campus structure template.

## Content workflow

1. Open `studio/index.html` locally, preferably through `python tools/serve.py`.
2. Edit buildings, floors, areas, photos, hotspots, arrows, motion, and welcome content through the UI.
3. Save a **Backup JSON** outside the public repository when it contains working/internal notes.
4. From **Publikasi**, export the visitor HTML.
5. Replace repository-root `index.html` with that exported HTML.
6. Commit and push to GitHub. A connected Vercel project can deploy the new `index.html` automatically.

The Studio draft itself is stored in browser IndexedDB; editing locally does **not** rewrite repository files automatically.

## Deployment

`.vercelignore` excludes the Studio, tests, source, and internal development files from the Vercel upload. The production deployment therefore exposes the visitor tour only.

## Credits

© Institut Sains dan Teknologi Terpadu Surabaya  
Created by Arustudio
