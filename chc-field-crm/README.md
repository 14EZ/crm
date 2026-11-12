# CHC Field CRM

A lightweight, mobile-first CRM for a remodeling / restoration project manager who works between the office and the field. It keeps every job, contractor, inspection, checklist and office form in one place instead of scattered across WhatsApp groups and phone notes.

It's a single HTML file with no build step, no server and no dependencies. It runs on GitHub Pages and can be installed on an iPhone home screen like an app.

## Features

**Projects**
- Each project is named by its address and has a client, phone, salesperson, job type, status and crew.
- Three statuses: **Active**, **Pending** (yellow: waiting on a city permit, insurance, the client, materials or financing) and **Completed**.
- Picking a job type (bathroom, kitchen, painting, concrete/asphalt, electrical, plumbing, water damage/insurance, flooring, new construction/ADU) loads:
  - a typical **materials** list
  - **watch-outs** to check during the job
  - a **completion / handover** checklist
- Features you can turn on or off per project: inspections log (type, date, result, corrections), files and photos, engineering plans.
- Timestamped activity log.
- One-tap call, WhatsApp update to the client (pre-written message), Google Maps / Apple Maps.
- **Tile calculator:** walls and floor in feet or feet + inches, subtract windows and niches, choose 10–20% waste. Gives tiles and boxes, and can save the result to the project log.

**Contractors**
- Profiles with phone, license #, skills, team, teammates, and active and completed jobs.
- Teams with a **job limit**. The app warns when a team or solo contractor is over its limit.

**Map**
- Active jobs on a drawn map of the Inland Empire (Riverside and San Bernardino counties): service cities and main freeways.
- Tap a pin to open the project. "Move pins" lets you drag a pin to the exact spot.
- Build today's route by ticking jobs in order, then open the whole route in Google Maps.

**Office**
- Personal folder for forms and files (completion form, finance form, per-city inspection checklists, material lists).
- Reusable in-app checklists you can share via WhatsApp.
- Editable job-type templates.
- Settings (your name and company for messages), plus backup export / import.

## Where data is stored

| Running it… | Data | Files & photos |
|---|---|---|
| From GitHub Pages / any web host | Browser `localStorage` on that device | Browser IndexedDB on that device |
| As a Claude artifact | Private cloud database (synced across devices) | Artifact file storage |

When it's hosted on GitHub Pages, **data stays on the device you use it on.** Nothing is sent to a server. Use **Office → Backup → Export backup** regularly. The backup is a JSON file that includes your uploaded files, and you can import it on another phone or computer.

> ⚠️ Backups contain client names, addresses and phone numbers. Don't commit them to the repository. `.gitignore` already excludes `field-crm-backup-*.json`.

## Publish on GitHub Pages

1. Create a new repository on GitHub (for example `chc-field-crm`).
2. Upload the contents of this folder (`index.html`, `manifest.webmanifest`, `sw.js`, `icons/`, `README.md`, `LICENSE`, `.gitignore`, `docs/`), or push with git:
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/<your-username>/chc-field-crm.git
   git push -u origin main
   ```
3. In the repository go to **Settings → Pages**, set **Source: Deploy from a branch**, pick **main** and **/ (root)**, and save.
4. After a minute the app is live at `https://<your-username>.github.io/chc-field-crm/`.

### Install on iPhone
Open the link in **Safari** → tap **Share** → **Add to Home Screen**. It opens full-screen like an app and keeps working offline once loaded.

## Run locally
Open `index.html` directly in a browser, or serve the folder:
```bash
python3 -m http.server 8000
# then open http://localhost:8000
```
(Offline mode and home-screen install only turn on over `https://` or `localhost`.)

## Customizing

Everything lives in `index.html`. The constants near the top of the script are the easiest place to start:

| Constant | What it controls |
|---|---|
| `CITIES` | Service cities and their map coordinates |
| `FREEWAYS` | Freeway lines drawn on the map |
| `JOB_TYPES`, `DEFAULT_TEMPLATES` | Job types and their default materials / watch-outs / completion checklists (also editable in the app) |
| `SKILLS`, `PENDING_REASONS`, `INSPECTION_TYPES`, `FILE_CATS`, `OFFICE_CATS` | Pick-lists |

After changing cached files, bump `CACHE` in `sw.js` (for example `field-crm-v2`) so installed copies update.

## Known limitations / roadmap
- **No shared cloud sync on GitHub Pages.** Each device has its own data, and you move it with backups. Syncing would need a backend (for example Supabase or Firebase).
- **Map is schematic,** not a street map. A real map (Leaflet + OpenStreetMap or the Google Maps API) is a good next step for the hosted version.
- **Adding contacts to the iPhone:** "Copy contact" copies the name and number. Writing directly to iOS Contacts needs a native app.
- **WhatsApp:** uses `wa.me` links with pre-written messages. An automated bot would need the WhatsApp Business Cloud API and a server.

## License
MIT. See [LICENSE](LICENSE).
