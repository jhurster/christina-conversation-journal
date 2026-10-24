# Christina — Conversation Journal (Google Drive edition, v2)

A React/Vite installable web app. Google OAuth signs you in, and journal entries (JSON files) plus audio recordings are stored in **your own Google Drive** under a folder called `Christina Conversation Journal`. No Firebase required.

## One-time Google Cloud setup

1. Open https://console.cloud.google.com/ and create/select a Google Cloud project (this is **not** Firebase).
2. In **APIs & Services → Library**, enable **Google Drive API**.
3. In **Google Auth Platform** (or **APIs & Services → OAuth consent screen**), configure the app name and contact details. For a personal app, choose **External**, leave it in **Testing**, and add your Google email under **Test users**. Testing authorization can expire after 7 days; publishing and verification requirements depend on your scopes and audience.
4. In **Clients / Credentials**, create an **OAuth client ID** of type **Web application**. Add authorized JavaScript origins for local development (`http://localhost:5173`) and your future HTTPS site (`https://your-site.example`). You do not need a redirect URI for this token-popup flow.
5. Copy `.env.example` to `.env`, and replace the placeholder with your Web client ID. **This ID is public configuration, not a client secret. Do not put a client secret in frontend code.**
6. Run `npm install` then `npm run dev` to test locally. Run `npm run build` to generate the `dist/` folder for static HTTPS hosting (for example, Netlify, Vercel, or Cloudflare Pages). Add the same `VITE_GOOGLE_CLIENT_ID` environment variable to your hosting build configuration, and add your deployed site origin in the Google Cloud OAuth client.
7. On iPhone, open the HTTPS URL in Safari, then Share → Add to Home Screen. Other devices use the same URL and Google account.

## Permissions and privacy

- The app requests the Google Drive `drive.file` OAuth scope. This lets the app manage files it creates, including its dedicated journal folder, without broad access to your existing Drive documents.
- The app runs in your browser; it does not use a server to store notes. Google Drive holds your notes and recordings.
- OAuth access tokens are held only in page memory. If the token expires or the page reloads, sign in again. This is intentional to avoid persistent token storage.
- Do not share your Google account or the journal folder. Google Drive permissions govern who can access files.
- Files are not end-to-end encrypted by this app; Google's normal Drive protections apply.
- Google Cloud OAuth setup is separate from any Google Drive connector already connected inside ChatGPT.

## Usage

- **Text:** enter a thought, select a category, and Save note.
- **Audio:** tap Record audio, allow microphone access, then Stop & save. Keep the page open until upload completes. Text in the editor is attached to the recording.
- **Search and categories:** filter the entries shown. Refresh to pull edits made from another device.
- **Export:** Notes exports Markdown; JSON exports metadata/text. Audio recordings must be downloaded separately, or directly from the Drive folder.
- **Edit/Delete:** changes sync to Drive. Deleting an entry also attempts to delete its associated recording.

## Important limitations

- This version is a **source-code deliverable, not a deployed website**. Google sign-in and Drive storage require the setup above.
- No transcription or automatic AI classification. Audio must be played or downloaded before sharing with ChatGPT.
- The app stores one JSON file per note, which is simple and human-accessible but slower than a database for very large journals.
- This app has not been end-to-end tested with a real Google account. Test with a sample note and recording before relying on it for important content.
- Avoid simultaneous edits to the same note on different devices; last save wins.
