# Road to 200

A mobile-first fitness tracker. Everything lives in one HTML file with no build step and no dependencies.

Tracks: daily weigh-in, the 3 fixed meals, free meal, water (1 gallon), a 1-mile walk, floor-only bodyweight workouts (A/B), up to 10 custom habits, a weight trend chart, a weekly report and pace / ETA to goal.

## Files

| File | What it is |
|---|---|
| `index.html` | The whole app: HTML, CSS and JavaScript |
| `manifest.webmanifest` | Lets phones install it as a home-screen app |
| `icon.svg`, `icon-180.png`, `icon-192.png`, `icon-512.png` | App icons |

## Run it locally

Any static server works. From this folder:

```bash
python3 -m http.server 8000
# or
npx serve .
```

Then open http://localhost:8000. In VS Code, the **Live Server** extension also works (right-click `index.html` → *Open with Live Server*).

Opening the file directly (`file://`) mostly works, but the manifest and icons need a server.

## Where data is saved

- **Standalone (this zip):** data saves to the browser's `localStorage`, under the key `roadTo200:v1`. That means it stays on **that one browser on that one device**. Clearing browser data erases it.
- **On claude.ai:** the same code detects Claude's hosted database and syncs across devices instead. Your claude.ai log does not carry over to this copy.

To sync across devices on your own hosting, replace the `localDb()` function in `index.html` with a real backend such as Supabase or Firebase. It only needs to provide two things:

- `collection(path).doc(id).set(data)`
- `collection(path).onSnapshot(callback)`

### Data shape

- Doc `settings`: `{ startWeight, goal, cal, protein, groceries: {key: bool}, habits: [{id, name}] }`
- Doc `dYYYY-MM-DD` (one per day): `{ date, weight, meals: {b, l, d}, free, walk, water, habits: {id: bool}, workout: { type: 'A'|'B', done, sets: { exerciseKey: [{r}, {r}, {r}] } } }`

## Put it on your phone

1. Host the folder anywhere static. The free options are Netlify Drop (drag the folder onto app.netlify.com/drop), GitHub Pages, Vercel or Cloudflare Pages.
2. Open the URL on your phone:
   - **iPhone:** in Safari, tap **Share → Add to Home Screen**.
   - **Android:** in Chrome, tap **⋮ → Install app** or **Add to Home screen**.

It opens full-screen with its own icon.

## Customize

Everything is at the top of the `<script>` in `index.html`:

- `DEF`: start weight, goal, calorie and protein targets
- `MEALS`: the meal plan
- `EX`: the workouts (rep ranges, cues and "harder" progressions)
- `GROCERIES`: the weekly grocery list
- `WATER_GOAL`: in oz
- `TRAIN_DAYS`: `[1, 3, 5]` means Mon / Wed / Fri
- `MAX_HABITS`: the habit limit
- Colors are CSS variables at the top of `<style>`. Dark mode follows the system setting.
