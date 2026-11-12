# mychat.freedomain — Firebase social chat starter

This is a mobile-responsive web app starter that uses the Firebase project configuration supplied by you.

## Included
- Email/password signup and login
- User profile records in Firestore
- User directory
- One-to-one real-time messages
- Admin dashboard UI for listing and banning accounts
- Firestore security rules
- Firebase Hosting configuration
- Installable-web-app manifest

## Important setup before launch

1. In Firebase Console, open **Authentication → Sign-in method** and enable **Email/Password**.
2. Open **Firestore Database** and create a database if you have not already.
3. Deploy the included `firestore.rules` before testing.
4. The admin dashboard requires a trusted Firebase custom claim named `admin`. It is deliberately impossible for a normal user to grant this claim to themselves.
5. To grant admin, use a trusted environment with Firebase Admin SDK, never put a service-account private key in this website.

Example Node.js admin claim script (run only on a trusted computer/server after installing `firebase-admin` and configuring Application Default Credentials):
```js
const { initializeApp, applicationDefault } = require('firebase-admin/app');
const { getAuth } = require('firebase-admin/auth');
initializeApp({ credential: applicationDefault(), projectId: 'my-chat-fe03c' });
(async () => {
  const uid = 'REPLACE_WITH_YOUR_ADMIN_USER_UID';
  await getAuth().setCustomUserClaims(uid, { admin: true });
  console.log('Admin claim set. Sign out and sign in again.');
})();
```

## Run locally
Install Node.js, then from this folder:
```bash
npm install -g firebase-tools
firebase login
firebase use --add
firebase emulators:start
```
For a quick Hosting deploy:
```bash
firebase deploy --only firestore:rules,hosting
```
Select `my-chat-fe03c` when asked to use your existing project. Do not deploy until you have reviewed the rules and tested them.

## Notes / current MVP limits
- This starter provides direct one-to-one text chat, not group chat, attachments, voice/video calls, push notifications, or end-to-end encryption.
- The current user directory reads up to 100 profiles. For a public production service, add pagination, abuse prevention, rate limiting, reporting/blocking, and privacy controls.
- The app uses Firebase's public web configuration. Firebase web API keys identify the project; Firestore Security Rules and Authentication protect data. Never put service-account credentials or private server secrets in client code.
- For Windows EXE and Android APK, package the shared web app using a maintained native wrapper/build workflow after the website has been tested. This ZIP is the web source, not precompiled installers.
- `mychat.freedomain` is a project/brand label, not a registered domain. You will need to register a domain or use a Firebase Hosting domain such as `my-chat-fe03c.web.app`.
