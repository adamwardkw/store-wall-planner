# Photo wall planner

Open https://adamwardkw.github.io/store-wall-planner/.

A single-file personal planning tool. No backend, API key, paid API, or employee account. The app code is public; imported photos and layouts are not uploaded to the repository.

## The flow

1. **Text the picture.** Take a photo of the wall and text it to Instinct. Employees text theirs to Adam or Instinct. There is no upload button, shared folder or account on this page.
2. Instinct reads the photo with a vision model, counts every position and sends back a map file (`wall-spot-map.json`) that also records **what is in each position right now** and which positions are empty.
3. **Import the map Instinct sent.** The **Wall blocks** view opens: the wall as a flat, front-on layout of straight rows and uniform spots. Consecutive products with no gap between them are grouped into one coloured block - same colour means one run. Red dashed cells are empty positions. Tap a block to select it, then **Label block** (the AI guess is only a starting point - label blocks yourself), **Split into singles** (the singles keep the block's colour so you can still see they belong together), **Park aside** (it waits below the wall), or tap an empty position to move it there. Tap an empty position first to add a **New single in gap**. Blocks move within and across rows wherever enough consecutive empty spots are open. The photo views (**Photo overlay**, **Clean model**) remain for checking against the real wall. **Correct what is there** still fixes the recorded current contents; **Rebuild blocks from current wall** re-derives the blocks after corrections. The "On the wall now" table lists each run, best-guess names and the empty stretches, and prints with the plan.
4. Enter brand, item label, colour and count, then **Make line**. Select the line and tap its first position, or drag it onto the wall. Ten products become five, a one-position gap, then five: 11 positions. Distinct lines also need one empty position between them. A line that does not fit or overlaps another line is not placed. A line placed over current stock replaces it in the plan. The app says how many current products it covers.
5. **Clean model** removes the photo, leaving the positions, current contents and planned lines. Zoom to 2x or 3x on a phone. **Print / PDF** prints the wall view, the placement plan and the current-contents table. **Export plan** saves a full backup including the photo.

A photo on the device can be opened (Choose wall photo, Take a picture) as a backdrop. It is only for viewing and for manual marking. Manual **Mark spots** is a backup if no map is available. Opening a new photo clears the open plan.

The app saves the open plan in local browser storage. Storage failure is shown with an export warning. Clearing browser data removes that device's saved plan. Keep an exported backup. A downloaded `index.html` works offline; the hosted page needs to have loaded before it can be used without a connection.

The older row and measured planners remain under **Other modes**. Their saved state is separate and unchanged. Use **Photo wall** to return.

## Map format, version 1

```json
{
  "format": "wall-planner-photo",
  "version": 1,
  "name": "Shop wall",
  "photo": null,
  "shelves": [
    {
      "id": "left-1",
      "label": "Left row 1",
      "y": 0.4,
      "yEnd": 0.42,
      "spots": [0.1, 0.13, 0.16, 0.19, 0.22, 0.25, 0.28, 0.31, 0.34, 0.37, 0.4]
    }
  ],
  "lines": []
}
```

- Coordinates refer to the full original photo, not a crop. Origin is the top-left. Coordinates are fractions, not pixels.
- Each `shelves` entry is one fixed horizontal run, often one wall section within a row. This describes what is visible in the photo; it does not create shelving.
- `spots` lists x-centres from left to right. Allowed range: 0.02 through 0.98, with at least 0.007 between neighbours. At most 120 positions per run and 60 runs.
- `y` is the product bottom/baseline at the first spot, between 0.05 and 0.98.
- Optional `yEnd` is the baseline at the last spot; the app interpolates across an angled shelf. Without it, `y` is constant.
- Optional `ys` is an array of one baseline per spot and takes precedence over `yEnd`. It must have the same length as the already-sorted `spots` array. This allows more detailed perspective correction.
- `photo: null` keeps the already-open photo during import. A full export instead includes `photo: {"dataUrl":"data:image/jpeg;base64,...","width":1536,"height":1152}`. PNG and WebP data URLs also work. Image data URLs are limited to 4.5 million characters; map files to 6 MB.
- Optional `contents` on a shelf records what is there now. It is an array with exactly one entry per spot, in the same order as `spots`. Each entry is `null` (empty position) or `{"label":"best-guess product","color":"#rrggbb","conf":"high"|"low"}`. `low` means a guess that needs a visual check. Maps without `contents` still import as before and show no filled/empty state. The top-level optional `mappingNote` (up to 1200 characters) is shown under the wall and prints with it.
- `lines` may be empty for a new map. A populated line has `id`, `brand`, `item`, `count` (1-100), `color` (hex), `shelfId`, and zero-based `start`. Unplaced lines use `shelfId: null, start: null`. The app calculates five-unit runs and reserved gaps from those fields. Imports with overlapping runs are rejected.
- Optional top-level `blocks` records the flat working layout: `{id, shelfId, start, len, label, color, group}`. Parked blocks use `shelfId: null, start: null`. `group` links singles that were split from one block (they keep its colour). The app derives blocks from `contents` when a map has none, so exported plans round-trip and new maps work unchanged.

Do not commit real shop photos, employee photos or private maps to this public repository. The photo-share handoff and the `wall-mapping-request` file from earlier versions were removed.
