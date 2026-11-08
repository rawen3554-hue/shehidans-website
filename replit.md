# Replit deployment notes — Britain, 30 Shehidans

This is the independent website source. It is not a Higgsfield app.

## Start command
Run `npm start`. The included `server.mjs` serves the `public/` folder on `0.0.0.0:$PORT` and provides `/healthz` for health checks.

## Requirements to preserve
- Public browsing works without visitor accounts.
- Keep Home, Community, Directory, Areas, and Games and the existing Windows 7/Aero-inspired style.
- Keep the Kurdish prize/troll game, wheel, and the four-step hidden Owner Mission exactly as implemented.
- Community posts, directory entries, and area descriptions must sync between devices through a persistent database.
- Owner create/edit/delete actions must be authorized server-side. The hidden mission is only an easter egg, never authentication.
- Visitors must not be required to sign in.
- Keep public SEO metadata, sitemap, robots.txt, favicon, web manifest, and service worker.
- Do not add Higgsfield or any private-app wrapper.

## Backend status
The current version contains a Firebase client adapter and Firestore security rules. `public/firebase-config.js` contains placeholders, so shared posting/editing is not yet enabled. Until a backend is configured, the site opens in preview mode. A Replit full-stack deployment should use a persistent database and a server-verified owner session, or complete the existing Firebase setup with the user's authorized project. Never put service-account keys or owner secrets in files under `public/`.
