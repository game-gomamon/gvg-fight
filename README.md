# Guild Fight Counter Book

A lookup page for Etheria Restart guild fights. Pick the enemy Animus you're
facing, get back every recorded lineup that contains them, plus the recommended
counter team and the play note.

## What's in here

| File | What it does |
| --- | --- |
| `index.html` | The whole site. No build step, no framework. |
| `data.json` | Lineups + roster. This is what the page reads. |
| `assets/animus/`, `assets/shell/` | Portraits pulled out of the workbook, as WebP. |
| `build.py` | Regenerates `data.json` and the portraits from the workbook. |
| `etheria_restart.xlsx` | The source workbook. |
| `netlify.toml` | Netlify build + caching config. |

## How the search works

The three slots are a **set**, not a sequence. Selecting Luvia, Lowan, and Lily
returns every lineup whose enemy side contains all three, no matter what order
they sit in — so a row stored as `Lily / Lowan / Luvia` still matches, and it
displays in the order the sheet recorded it. Matched Animus get a red ring so
you can see at a glance where they sit in each lineup.

Fill one slot to see everything that Animus appears in, two to narrow, three for
an exact team. The same Animus can't be picked twice; already-used ones are
greyed out in the picker.

The picker splits the roster into **In the book** (Animus that appear on the
enemy side of at least one recorded lineup) and **Not recorded yet**, so you're
not hunting through 78 portraits for the handful that matter this week.

## The weekly update

Edit the `Guild_fight` sheet, then pick whichever path matches how you deploy.

### If the site is connected to a Git repo

Commit the updated `etheria_restart.xlsx` and push. Netlify runs `build.py`
during deploy and the site picks up the new lineups. Nothing else to do.

### If you drag-and-drop the folder onto Netlify

Run the build yourself first, then drag the folder to
[app.netlify.com/drop](https://app.netlify.com/drop):

```bash
pip install -r requirements.txt   # first time only
python3 build.py
```

### If you just want to check something quickly

Open the deployed site, click **Load .xlsx** at the bottom, and pick the updated
workbook. The lineups refresh immediately in your browser — nothing is uploaded
and nothing is deployed. It's a preview, so it disappears on reload.

## Adding new characters

`build.py` reads names from column A (Animus) and column D (Shell) of the
`Information` sheet, and pulls the in-cell picture beside each one. Add a
character there with its portrait and it flows through automatically.

A character used in `Guild_fight` but missing from `Information` still works —
it just shows a blank tile with the name under it, which is a useful signal that
a portrait is missing.

## Running it locally

`index.html` fetches `data.json`, so opening the file directly with `file://`
won't load the data. Serve the folder instead:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```
