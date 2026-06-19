[ProjectFlow-Mobile-Setup.md](https://github.com/user-attachments/files/29138412/ProjectFlow-Mobile-Setup.md)
# Project Flow — Mobile companion: setup

`ProjectFlow-Mobile.html` is a tablet/phone companion. It reads and writes the **same `project.json`** files as the desktop app, via the **Google Drive API**. It can view every tab and add/edit deliverables, actions, risks, RFIs, minutes and notes/specs. It deliberately can't create projects or change settings (categories/milestones come from the project).

There are three one‑time jobs: put the portfolio on Google Drive, create a free Google OAuth Client ID, and host the HTML file. Then you sign in on the tablet and go.

## 1. Put the portfolio on Google Drive (laptop)
1. Install **Google Drive for desktop**. It adds a drive (e.g. `G:\My Drive`).
2. Create/move your portfolio folder there, e.g. `G:\My Drive\ProjectFlow`, with one subfolder per project (each containing `project.json`).
3. Right‑click that folder → **Offline access → Available offline** (reliable file access for the desktop app).
4. In the desktop `ProjectFlow-V2.html`, click **Open Portfolio** and pick `G:\My Drive\ProjectFlow`. Confirm it saves there.

## 2. Create a Google OAuth Client ID (free, ~10 min)
1. Go to **console.cloud.google.com** → create a project (any name).
2. **APIs & Services → Library →** search **Google Drive API → Enable**.
3. **APIs & Services → OAuth consent screen →** User type **External** → fill app name + your email → **Add users** and add your own Google account as a **Test user** → Save. (Testing mode needs no Google verification for your own use.)
4. **APIs & Services → Credentials → Create credentials → OAuth client ID →** Application type **Web application**.
   - Under **Authorized JavaScript origins**, add the exact URL where you'll host the file (from step 3 below), e.g. `https://yourname.github.io`. (Origin only — no path, no trailing slash.)
   - Create, then **copy the Client ID** (looks like `…apps.googleusercontent.com`).

## 3. Host the HTML file (free)
Easiest is **GitHub Pages**:
1. Create a free GitHub account + a new public repo.
2. Upload `ProjectFlow-Mobile.html` (rename to `index.html` for a clean URL).
3. Repo **Settings → Pages →** deploy from the main branch → it gives you `https://yourname.github.io/repo/`.
4. Put that origin (`https://yourname.github.io`) into the OAuth client's Authorized JavaScript origins (step 2.4) if you didn't already.

(Any static HTTPS host works — Netlify, Cloudflare Pages, etc. It must be HTTPS for Google sign‑in.)

## 4. First run on the tablet
1. Open the hosted URL in **Chrome** on the tablet.
2. Enter your **Client ID** and the **portfolio folder name** (e.g. `ProjectFlow`) → **Save settings**.
3. Tap **Sign in with Google** → approve. In testing mode Google shows an "unverified app" notice — it's your own app, choose **Continue**.
4. Pick a project from the top dropdown, browse the tabs, tap **＋ Add** to capture inputs.
5. Chrome menu → **Add to Home screen** for an app icon.

## How saving / safety works
- Every add or edit writes the whole `project.json` straight back to Drive, and appends to the matching `…-log.md` — same as the desktop.
- Before overwriting, it checks whether the file changed on Drive since you opened it; if so it warns you (overwrite vs reload). So the only thing to avoid is editing the *same* project on the laptop and tablet in the *same moment*.
- Needs internet (it talks to Drive live). Refresh (⟳) reloads the active project.

## Notes / limits
- Scope requested is full Drive access so it can locate your portfolio and projects — it's your own account/data.
- If you change the hosting URL later, update the Authorized JavaScript origin to match.
- Couldn't be tested end‑to‑end from here (sign‑in needs your account), so expect a small round of fixes once you're connected — send me any error text and I'll sort it.

## Quick test checklist (once connected)
- Sign in → project list populates → pick one.
- Deliverables show, grouped by milestone, with the late banner if anything's overdue.
- Tap a status pill → it advances and persists after Refresh.
- Add an Action → it appears, and reopening the project (or desktop) shows it.
- Add Minutes with an "actions arising" line → a new action appears too.
