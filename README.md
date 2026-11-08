# Britain, 30 Shehidans — standalone public website

This is an independent HTML/CSS/JavaScript website. It has **no Higgsfield integration** and no private-app wrapper. It includes a Node server entry point for standard app hosts. The current data adapter uses Firebase/Cloud Firestore, with public reads and owner-only publishing once configured.

## What is built

- Responsive Windows 7/Aero-inspired interface.
- Public Home, Community, Directory, Areas and Games sections; visitors do not need accounts.
- Three selectable alley pages, including Second Alley — Mr Bean Fara.
- Prize-button monkey troll and animated surprise wheel.
- Four-step hidden Owner Mission: second crown press = ancient clock, third = wizard, fourth = fire; then “Claim ownership” appears in Games and reveals the black orb on Home.
- Optional Google sign-in for profile. Profile color is randomized per session; a rare animated fish-tank avatar is included.
- Optional Gmail connection, requested only after the visitor presses “Connect Gmail”. The code requests only `gmail.readonly` and `gmail.send` scopes; it does not request Drive, Contacts, Calendar or other Google data.
- Real-time Cloud Firestore reads for community news and directory entries; owner-only create/delete UI.
- Firestore security rules enforce the owner UID server-side. The hidden sequence is **not** a security boundary.
- SEO title/description, structured website metadata, canonical URL placeholder, sitemap, robots.txt, favicon, PWA manifest and service worker.

## Google free-tier capacity (current published quotas)

- Firebase Hosting: up to 10 GB stored site content and up to 10 GB/month data transfer at no cost.
- Cloud Firestore: 1 GiB stored data, 50,000 document reads/day, 20,000 writes/day, 20,000 deletes/day and 10 GiB/month outbound transfer in the free quota.
- Cloud Storage for Firebase file buckets require the Blaze billing plan. This project does not use that separate bucket; text/news/directory data goes into Firestore.

These limits are Google quotas, not a promise of unlimited storage. Usage above free quotas can require billing or cause service restrictions depending on the product/plan.

## Run the website

Run `npm start` on a Node.js host. For the shared Community, Directory and Areas data, the Firebase project still needs its web configuration, Authentication and Firestore rules configured as described below. Without that backend configuration, the public pages run in preview mode.

## One-time Firebase project setup required before shared publishing

A public website cannot be published into somebody's Google account without that account authorizing a Firebase project. This source package is ready, but it does not contain your Google credentials or Firebase project ID.

1. In Firebase Console, create a project (display name: **Britain, 30 Shehidans**). Pick a globally unique Project ID; the public default address will be `https://PROJECT_ID.web.app/`.
2. Add a Web App and copy its Firebase web config into `public/firebase-config.js`. The web config is public; never place a service-account key or private credential in browser code.
3. Enable **Authentication → Google**. Add the final `PROJECT_ID.web.app` host to Authorized domains if Firebase does not add it automatically.
4. Create the default **Cloud Firestore** database. It is used for the public news and directory collections.
5. After the owner signs in once, copy that account UID from Firebase Console → Authentication → Users. Replace `OWNER_UID_GOES_HERE` in both `public/firebase-config.js` and `firestore.rules` with that UID. The hidden Owner Mission only reveals the workspace UI; Firestore rules are the actual write protection.
6. Enable the Gmail API in the linked Google Cloud project only if you want the optional Gmail read/send feature. Configure an OAuth consent screen and verification as required by Google. Gmail permission is requested only after a visitor clicks Connect Gmail, and only for `gmail.readonly` + `gmail.send`.
7. Install Node.js, then in this folder run `npm install`, `npx firebase login`, and `npx firebase use --add` to select your project.
8. Run `npm run deploy`. The deploy script reads the selected Firebase project ID and writes the matching canonical URL, robots.txt and sitemap before publishing hosting and Firestore rules.
9. Open `https://PROJECT_ID.web.app/` in a private/incognito browser to verify it works without signing in. Sign in with the owner's Google account, copy the Account ID shown in the profile, put that UID into `ownerUid` in `public/firebase-config.js` and `firestore.rules`, then run `npm run deploy` again.
10. Submit `https://PROJECT_ID.web.app/sitemap.xml` in Google Search Console. Google discovery/indexing is not instant.

## Notes

- The user-facing website is public without sign-in; Google sign-in is optional.
- Firestore public reads are intentionally limited to `community`, `directory` and `areas`. Other collections are denied by default.
- Only the owner UID configured in `firestore.rules` can create/update/delete community, directory and area entries. Keep the rule in place; do not replace it with public writes.
- Gmail access tokens are held in browser memory for the current visit and are not stored in Firestore or local storage.
- This project deliberately does not store private Gmail content in its own database.
- The `Areas` page does not invent a map. Add the accurate map or public map pins only when the owner supplies them.
