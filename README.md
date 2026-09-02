# Trainers App — with server

This version of Trainers App runs via a small Node.js server. Two things change as a result:

1. **The app is hosted** — you open it via a URL (e.g. `http://localhost:3000`) instead of opening the file locally.
2. **Data is stored centrally** — training plans, nutrition, planner and workout history live in `data/db.json` on the server, instead of separately per device in the browser. Open the app on your phone and your laptop, and you'll see the same data everywhere.

## Running locally

Requires: [Node.js](https://nodejs.org) (version 18 or higher).

```bash
cd vormkracht-server
npm install
npm start
```

Then open `http://localhost:3000` in your browser. You'll be asked to log in — see below.

## Login credentials

Default: **username `trainer`, password `vormkracht123`**. Change this — especially before going live on the internet — via environment variables:

**Locally (PowerShell on Windows):**
```powershell
$env:APP_USER="yourusername"
$env:APP_PASS="yourpassword"
npm start
```

**On Render:** go to your service → "Environment" tab → add `APP_USER` and `APP_PASS` with your own values, and save (Render restarts the service automatically).

## Using it on your phone (same wifi)

Find the local IP address of the computer running the server (e.g. `192.168.1.23`) and open `http://192.168.1.23:3000` on your phone. This works for plans, nutrition, and planner — **but the camera/barcode scanner won't work here**, since browsers only allow camera access over `https://` or `localhost`. For scanning on your phone you need real https hosting (see below).

## What's included

- **Training plans** with a workout mode (rest timer, session clock, set-by-set logging).
- **Nutrition tracking**, including barcode scanning (via Open Food Facts) and manual entry.
- **Planner** — a calendar combining training days and nutrition logs.
- **Client profiles** — switch the "Viewing" selector in the header (or the Clients tab) between "Yourself" and any client you add. Plans, nutrition, planner, workout history and body weight are all kept separate per client.
- **Exercise library** — exercise names you've used before show up as suggestions while building a plan, auto-filling the muscle group and joint movement.
- **Progress tab** — personal records (heaviest logged weight per exercise) and a progress chart per exercise, plus a body-weight chart.
- **Share links** — the "🔗 Share Link" button on a plan generates a public, no-login link (`/share.html?token=...`) you can send to a client so they can view that plan.
- **PDF export** — the "🖨️ Export PDF" button opens your browser's print dialog with a clean, printable version of the plan (choose "Save as PDF" there).
- **Installable app (PWA)** — visiting the site in a mobile browser offers an "Add to Home Screen" / "Install" option, so it opens like a native app. The core screens keep working offline; anything that needs the server (saving, syncing, barcode lookups) still needs a connection.

## Reachable anywhere + https (for scanning on your phone)

To reach the app from any device with a working camera scan, it needs to be hosted somewhere with an https address. This server (Express + Node) can be deployed on most hosting platforms. A few options that handle https automatically:

- **Render.com** — free tier, connect a GitHub repo or upload the folder, "Web Service" with start command `npm start`.
- **Railway.app** — similar, also easy to connect to a repo.
- **A VPS with Caddy or nginx** in front — if you'd rather self-host.

In that case, put the `vormkracht-server/` folder (with `server.js`, `package.json`, `public/`) in a GitHub repo and follow the deploy steps for your chosen platform. I can help you with that once you've made a choice.

## Good to know

- There's a simple login on it (username + password, see above) — change the default values before making the app public.
- If the server is unreachable, the app still works (data stays in that session's memory), but nothing gets saved until the connection comes back. At the top of the app you'll see a status line ("● Connected to server" / "● Not connected").
