# Ronix Studios – Script Completion Tracker

A static site that lists Roblox games (10K+ CCU) that don't yet have a Ronix script. The script author marks a game as finished by uploading a proof image; the checkmark only appears once an image is attached.

## What it does

- Lists 54 Roblox games with 10K+ concurrent users that aren't covered by the existing Ronix script library.
- Each game card has a link to Roblox, a CCU badge, and an "Upload proof image to finish" button.
- A game is only marked **Finished** once a proof image is uploaded. No image, no checkmark.
- Progress persists in the browser via `localStorage`.
- Export/import progress as JSON so you can back up or move it between devices.
- Progress bar at the top shows X / 54 completed.
- Filter by All / Pending / Finished, plus text search.

## Hosting on GitHub Pages

1. Create a new public GitHub repository (e.g. `ronix-tracker`).
2. Upload `index.html` (and this `README.md`) to the repo's root.
3. Go to **Settings → Pages**.
4. Under **Source**, select `Deploy from a branch`, then pick `main` and `/ (root)`. Save.
5. Wait ~30 seconds. Your site will be live at `https://<your-username>.github.io/ronix-tracker/`.

That's the entire deploy. No build step, no backend.

## How the "proof image" requirement works

- Clicking the upload button on a card opens the file picker.
- After selecting an image, it's read locally as a base64 data URL and stored in `localStorage` along with a timestamp and the original filename.
- The card flips to the "Finished" state and shows a thumbnail of the proof.
- You can view the full image in a modal or remove the proof to reset that game to pending.
- Images must be under 4MB each (browser storage is limited).

## Backing up / sharing progress

- **Export progress** downloads a `ronix-tracker-progress.json` file containing all proof images and timestamps.
- **Import progress** loads a previously exported JSON back into the site.
- **Reset all** clears every entry.

Use Export + Import to move your progress between devices, or commit the JSON file to the repo as a record.

## If you want truly public, shared progress

`localStorage` is per-browser-per-device. If multiple people need to see the same progress:

1. After uploading proof images, click **Export progress**.
2. Commit the resulting `ronix-tracker-progress.json` and the proof images into the repo.
3. Have the site load that JSON on startup (small edit to `index.html`'s `loadState()` function — fetch the committed JSON file and merge it).

For a tighter "can't game it" flow, consider using GitHub Issues: one issue per game, close it with an image attachment as proof. The static site can read the Issues API to reflect state. Not included here because it requires a GitHub token to write.
