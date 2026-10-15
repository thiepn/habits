# Habits

Private, mobile-first habit tracking web application. Migrated from Daymark v5 under a new display name, without changing the underlying schema-v2 habit/history format.

## Contents

- `index.html` — installable PWA entrypoint
- `daymark-core.js`, `daymark-ui.js`, `daymark-storage.js` — original tested logic (internal names intentionally preserved)
- `manifest.webmanifest`, `sw.js`, `icons/` — offline app shell
- `habits-standalone.html` — standalone, single-file alternative
- `tests/` — deterministic regression tests

## Run locally

```bash
python3 -m http.server 8080
```

Open `http://localhost:8080/`. Node 18+ can run `npm test` without installing dependencies.

## Deploy

This is a static application. For Cloudflare Pages: connect `thiepn/habits`, use no framework, an empty build command, and `/` as the output directory. Prefer a dedicated `habits.thiepn.dev` HTTPS host. Do **not** publish to production before a real-origin migration, IndexedDB reload, service-worker/offline and mobile check. Deploying is not performed by this package.

## Data migration and privacy

- **Historical data authority remains `schemaVersion: 2`.** Keep original storage namespaces `daymark-habits-v2`, `daymark-habits-v1`, IndexedDB `daymark-v3` and the original internal engine names until a tested explicit data migration is ready. Renaming visible branding does not affect scores, revisions, routine planning or past entries.
- Browser storage is **origin-scoped**. Data from an old origin or `file:` URL does not automatically appear on `habits.thiepn.dev`.
- Before switching origins, export the complete JSON backup from the old app; then import it into Habits. Check counts, history, routines and reflection entries before relying on the new copy.
- Backups contain sensitive habit notes and history. Treat them as private.
- In-app reminders run only while the page is open; background notifications are **not guaranteed** after closing the app.
- No authentication, tracking, external API, cloud sync or paid service is included.

## Known qualification gaps

Real-origin installed PWA offline reopening, cross-reload IndexedDB, notification reliability and cross-tab conflict resolution have not been established by the prior in-document Chromium tests. Never present these as verified.

## Integration boundary

Habits is the canonical habit engine. THIEPN Hub may link to it or later consume an explicitly opted-in read-only progress summary. Avoid creating another competing habit store in Hub.