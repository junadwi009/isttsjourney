# Institut STTS Virtual Campus Tour

Virtual campus tour for **Institut Sains dan Teknologi Terpadu Surabaya**, created by **Arustudio**.

## Repository layout

- `index.html` — public visitor tour; use this as the Vercel entry point.
- `studio/index.html` — self-contained no-code Campus Studio editor/CMS for local authoring.
- `ISTTS-Template.campus.json` — initial 5-building campus structure.
- `docs/QA.md` — v2.0 validation notes.
- `.vercelignore` — keeps the editor and internal files out of the public Vercel deployment.

## Content workflow

1. Clone/download this repository.
2. Open `studio/index.html` locally.
3. Manage buildings, floors, areas, photos, hotspots, arrows, motion, and the welcome page through the UI.
4. Save a **Backup JSON** separately so the editable project is not lost.
5. From **Publikasi**, export the visitor HTML.
6. Replace the repository-root `index.html` with the exported visitor HTML.
7. Commit and push. A connected Vercel project can redeploy automatically.

The Studio draft is stored in browser IndexedDB. Editing in the local CMS does **not** directly rewrite files in GitHub.

## Initial tour structure

The template currently starts at **Gedung E → Lantai 1 → Resepsionis**. The start room is configurable in the Studio.

## Deployment

For Vercel, import this repository and use the repository root. The public deployment only needs `index.html`; `.vercelignore` excludes the local Studio and internal files.

## Credits

© Institut Sains dan Teknologi Terpadu Surabaya  
Created by Arustudio
