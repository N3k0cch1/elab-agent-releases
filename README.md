# elab-agent

A desktop companion for [elabFTW](https://www.elabftw.net/) — upload experiment data, manage entries, and capture live bench notes from your phone, all without touching the elabFTW web interface.

---

## What it does

**elab-agent** sits between your raw experiment files and your elabFTW electronic lab notebook. Drop a Word document, a PDF, a batch of images, or any other file; the app parses documents, detects experiment IDs, shows you a preview, and uploads everything to the right elabFTW entries in the correct order.

A single download — pick a mode from the launcher every time you open it:

| Mode | What you get |
|------|-------------|
| **Upload Only** | Word, PDF, and image upload — useful for shared lab installs |
| **Full Version** | Everything in Upload Only, plus: arbitrary file attach, live bench companion, experiment creation and deletion |

---

## Download

Go to the [**Releases**](../../releases) page and grab the file for your platform:

| Platform | File | Notes |
|----------|------|-------|
| Windows | `elab-agent-x.y.z-x64-setup.exe` | Installer with wizard |
| macOS (Apple Silicon) | `elab-agent-x.y.z-arm64.pkg` | M1/M2/M3+ Macs |
| macOS (disk image) | `elab-agent-x.y.z-arm64.dmg` | Drag to Applications |
| Linux | `elab-agent-x.y.z-x86_64.AppImage` | Make executable, run |
| Linux | `elab-agent-x.y.z-amd64.deb` | run dpkg to install |

> **macOS is Apple-Silicon-only** (arm64) as of v2.5.0 — there is no Intel (`x64`) build. Intel Macs are not supported by a native package.

---

## Installation

### Windows

1. Download `*-setup.exe` and double-click it.
2. The installer will show a progress bar — click **Install**, wait, then **Finish**.
3. Find **elab-agent** in your Start Menu or on your Desktop.

### macOS

Working with the `.pkg` installation:

1. Download the `arm64` `.pkg` (Apple Silicon — M1/M2/M3+).
2. Double-click → click through the 3-step installer.
3. Open **elab-agent** from your Applications folder.

> If macOS says the app is from an unidentified developer:
> right-click the file → **Open** → **Open** again in the dialog.
>
> If macOS says "Apple could not verify "`elab-agent-*.pkg`":
> Click **Done** → Open **System Settings** → Go to **Privacy & Security** → Go down to **Security** → Click to **Open Anyway** ("`elab-agent-*.pkg` was blocked to protect your Mac")

Working with the `.dmg` installation:

1. Mount the image.
2. Drag and place the elab-agent into the `/Applications` folder.
3. Open **elab-agent** from your Applications folder.

> If macOS says "App is damaged and can't be opened":
>
> ```bash
> xattr -cr /Applications/elab-agent.app
> ```

### Linux (AppImage)

```bash
chmod +x elab-agent-*.AppImage
./elab-agent-*.AppImage
```

### Linux (.deb)

```bash
sudo dpkg -i elab-agent-*.deb
# then launch from your application menu or:
elab-agent
```

---

## Updating

From **v2.5.2** the desktop app checks the releases feed on launch and shows an in-app banner when a newer version is available:

- **Windows (installer)** and **Linux (AppImage)** — the update downloads and installs in place, then the app restarts.
- **Linux (`.deb`)** — in-place update isn't supported; the banner links you to the latest release to install manually.
- **macOS** — builds are currently unsigned, so the update downloads but Gatekeeper may require you to open it manually (see the macOS notes above).

You can always download the newest build directly from the [Releases](../../releases) page.

> Earlier releases (≤ v2.5.0) shipped without an update feed, so auto-update first becomes available **from v2.5.2 onward** (v2.5.1 was never published).

---

## First launch — mode selection

When you first open elab-agent you will see the launcher:

- **Upload Only** — Word, PDF, and image upload. Good for labs with a shared install where bench-mode capture isn't needed.
- **Full Version** — all features, including the bench companion and experiment creation/deletion tools.

Once a mode is started, the **switch** button in the top-left corner of the landing page lets you return to the launcher to change modes.

---

## Setup wizard

The first time you pick a mode, a setup wizard walks you through four steps:

1. **Server** — enter your elabFTW server URL and probe it to confirm it is reachable.
2. **Template & resource** — pick the experiment template and resource category to use.
3. **ID pattern** — define the experiment ID format for your lab (e.g. `TFA x_yyy` → matches `TFA 1_001`).
4. **Markers** *(optional)* — set start/end keywords used to group pages in PDF notebooks.

Your settings are saved locally. You can reconfigure at any time via **Settings → Reconfigure**.

---

## Resource category

The resource category determines which template is used when creating a linked resource entry. Your active category is shown on the landing page. Click **change** next to it to pick a different one from the list fetched from your elabFTW server — the change takes effect immediately for all subsequent uploads without requiring a full reconfigure.

---

## Uploading files

### Word (.docx)

Drop a Word document onto the upload zone. elab-agent splits it into pages at section breaks, detects the experiment ID on each page, and extracts ChemDraw schematics automatically.

You get a page-by-page preview with:
- Detected ID (editable if wrong)
- Skip toggle to exclude pages you don't want to upload
- Duplicate warning if the ID already exists in elabFTW; click **Compare with existing** to see a side-by-side diff of the existing elabFTW body vs the new one before uploading
- AI tag suggestions (if an Anthropic API key is configured)

Click **Upload all** or use keyboard shortcut `Ctrl+Enter` to upload everything at once.

### PDF

Drop a PDF. Each page is rendered as an image, experiment IDs are detected, and pages are grouped into entries. Multi-page groups are stitched into a single inline image.

- **Split / Merge** — combine or split page groups with one click.
- **Drag to reorder** — drag cards to change the upload order.
- **Per-card upload** — upload individual entries without uploading the entire batch.

### Images

Drop one or more images (JPG, PNG, TIFF, WEBP, HEIC). Each image is matched to an experiment entry. Images go to the resource entry, not the experiment body.

Each image card has an **✏ Annotate** button that opens a full-screen drawing tool — pen, arrow, eraser, 7 colours, undo, reset. The annotated PNG replaces the original in the upload queue.

### Extra files — Full Version only

After uploading a Word or PDF document (or by dropping any unrecognised file), the **Upload extra files** panel appears. Use it to attach arbitrary files (raw data, spectra, spreadsheets, etc.) directly to an experiment's resource entry without parsing or preview.

- **Link to existing** — search for an experiment by ID or title and attach files to its resource.
- **Create new** — create a new experiment entry and attach the files in one step.

A per-file progress bar tracks each upload. Partially failed batches show which files failed with a **↺ Retry failed** button.

### Batch uploads

Drop multiple files at once. A file queue appears at the top — navigate with arrow buttons or click the dropdown. Each file is processed independently.

---

## Keyboard shortcuts

| Key | Action |
|-----|--------|
| `j` / `k` | Next / previous entry |
| `s` | Skip current entry |
| `u` | Upload current entry |
| `Ctrl+Enter` | Upload all entries |
| `?` | Toggle this help |

---

## Bench companion — Full Version only

The bench companion lets you capture live notes, photos, and videos from your phone during an experiment, then upload everything to elabFTW in one step at the end.

### Starting a session

1. On the desktop app, go to **Bench** and start a new session (give it an ID and a title).
2. On your phone, open a browser and navigate to the URL shown on the screen (e.g. `http://192.168.1.x:3000/bench`). No app install needed.
3. Add steps as you go. Notes and photos are autosaved every 30 seconds.

### Features

- **Stopwatch** — time your reaction, add splits and laps to track each step.
- **Photos & videos** — captured inline; iOS HEIC images are converted automatically.
- **Offline mode** — if the network drops, an amber banner appears and notes are buffered until the connection is restored.
- **Auto-recovery** — open sessions survive crashes and reloads; the app offers to resume on next launch.
- **Duplicate detection** — entering an experiment ID that already exists warns you before you start recording into it.
- **Upload** — when the experiment is done, tap **Upload to elabFTW** from the desktop app.
- **PDF export** — closed sessions can be exported as a formatted PDF (header card, timeline, stopwatch table, embedded photos) from the History page.

---

## Upload history

The **History** tab keeps a log of everything uploaded this session:

- **Uploads** — files sent to elabFTW with status (success / failed), including extra file uploads.
- **Created** — entries created via the manual creation tool.
- **Deleted** — entries deleted via the bulk delete tool.
- **Bench** — bench sessions uploaded.

Failed uploads show a **↺ Retry** button that takes you back to the upload page with a hint about which file to drop again.

You can also **Export CSV** from either the Word or PDF review panel — choose "All entries" or "Uploaded only".

---

## Multi-server profiles

If you work with more than one elabFTW server, use **Profiles** in the top navigation bar:

1. Connect to a server and enter your API key normally.
2. In the nav bar, click **Profiles** → type a name → **Save**.
3. Switch between profiles with one click — no re-entering API keys.

API keys are stored only in your browser's local storage and never sent to any server.

---

## Managing experiments

The **Create / Delete** tab lets you:

- **Create** a new blank experiment entry manually (useful for setting up before a bench session).
- **Delete** one entry using the searchbar.
- **Bulk delete** — search your experiments, tick checkboxes, and delete several at once.

---

## Template selection

Each upload session can use a different elabFTW template. Click the **Template** dropdown in the review panel header to switch. Click the **👁** (eye) button next to a template to preview its content before selecting it.

---

## User identity

After entering your API key, your name is displayed in the navigation bar. This confirms which elabFTW account is active and that your key has the correct team membership.

---

## Appearance — themes + dark mode

Click the **☾ / ☀** button in the top-right corner of the nav bar to toggle dark mode. Your preference is saved.

In **Settings → Appearance** you can also switch the visual theme:

| Theme | Palette | Feel |
|-------|---------|------|
| **Warm Minimal** *(default)* | Coral · mint · lavender (warm triad) | Notebook-paper warm minimal |
| **Cobalt Precision** | Cobalt blue · spring lime · dusty rose (cool triad) | Crisp, scientific, blue-dominant |
| **Lab Companion** | Emerald · purple · amber (green-dominant triad) | Instrument readout, green-led vibe |

Each theme has independent light and dark variants. The chosen theme propagates to the launcher, loading splash, and log viewer windows as well.

---

## Advanced — AI tag suggestions

If you have an Anthropic API key, elab-agent can suggest tags for each experiment entry. Enter the key in the setup wizard or via **Settings**. Tag suggestions appear inline in the review panel and can be accepted or dismissed.

---

## Troubleshooting

### "elabFTW is not reachable"
- Check that your server URL is correct (include `https://`).
- Make sure your API key has not expired.
- If you are on a VPN, confirm it is connected.

### The app opens but shows a blank screen on Linux
- Try running with `--no-sandbox` if using the `.deb` package.
- Check the **Logs** button on the landing page for backend error messages.

### Connectivity pill — what each colour means
The pill in the top-right of the title bar shows your live link to elabFTW:

| Tone | Meaning |
|------|---------|
| Mint dot | elabFTW reachable; sub shows current latency |
| Amber pulsing dot | one or more requests in flight (uploads, syncs); sub shows count |
| Coral dot | elabFTW unreachable |
| Neutral dot | first check in progress |

Click the pill to open a popup with the server URL, version, last sync time, the most recent error (if any), and a recent-activity log.

## Report a bug

Click **Report a bug** in the title bar (the small life-buoy icon next to the dark-mode toggle). Type what happened, optionally attach the recent backend log, and **Send report**. 

The report is kept locally in `data/feedback.jsonl` so it isn't lost.

---

## About

Built for daily use in the Nanomaterials and Polymer Chemistry Lab, NAIST, Japan.
Backend: Python (FastAPI + PyInstaller). Frontend: React + Vite. Desktop: Electron.

[elabFTW](https://www.elabftw.net/) is open-source ELN software — this tool is an independent companion, not affiliated with the elabFTW project.

To report a bug, use the **Report a bug** button inside the app — it forwards privately to the maintainer's tracker, with optional log attachment.
