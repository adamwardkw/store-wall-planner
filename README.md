# Photo wall planner

Open https://adamwardkw.github.io/store-wall-planner/.

A single-file personal planning tool. No backend, API key, paid API, or employee account. The app code is public; imported photos and layouts are not uploaded to the repository.

## The flow

1. **Text the picture.** Take a photo of the wall and text it to Instinct. Employees text theirs to Adam or Instinct. There is no upload button, shared folder or account on this page.
2. Instinct reads the photo with a vision model, counts every position and sends back a map file (`wall-spot-map.json`) that also records **what is in each position right now** and which positions are empty.
3. **Import the map Instinct sent.** The flat **Your wall** view opens: straight rows and uniform spots. Consecutive products are grouped into coloured blocks. Tap a block to label it, split it into singles, park it below the wall, or move it by tapping an empty spot. Singles split from one block keep their shared colour and group. Blocks move within and across rows wherever enough empty spots are open.
4. **Check the empty spots.** The app makes a conservative first read of each empty position: orange means **likely missing**, grey means **true gap**, and red means **needs check**. Ambiguous spots stay red instead of being forced into a guess. Tap an empty position and use **Missing product**, **True gap**, or **Auto-read** to correct it. The photo overlay remains available for checking against the real wall.
5. **Place products with the pull bar.** Enter the optional brand, item label and colour, pull the slider to the number of spots, then **Make pull bar**. The bar waits below the wall. Tap the bar and an empty position; it lands as separate singles that share one colour group. The pull limit adapts to the longest run in the imported map. For strict shelf lines, open **Need runs of 5 with a gap?** and use **Make 5-gap line** to place runs of five with one empty spot between lines.
6. **Clean up and keep the plan.** Use **Correct what is there** for current-stock mistakes and **Rebuild blocks from current wall** after corrections. **Print / PDF** prints the wall view, placement plan and current-contents table. **Export plan** saves a full backup including the photo.

A photo on the device can be opened with **Choose photo** as a backdrop. It is only for viewing and manual marking. Manual **Mark spots** is a backup if no map is available. Opening a new photo clears the open plan.

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
- Optional `gapKinds` on a shelf records manual empty-spot reads as `"gap"` or `"missing"`, aligned with `spots`. Absent or other values mean the automatic read applies.
- `lines` may be empty for a new map. A populated line has `id`, `brand`, `item`, `count` (1-100), `color` (hex), `shelfId`, and zero-based `start`. Unplaced lines use `shelfId: null, start: null`. The app calculates five-unit runs and reserved gaps from those fields. Imports with overlapping runs are rejected.
- Optional top-level `blocks` records the flat working layout: `{id, shelfId, start, len, label, color, group}`. Parked blocks and pull bars use `shelfId: null, start: null`. `group` links singles that belong to one colour group. The app derives blocks from `contents` when a map has none, so exported plans round-trip and new maps work unchanged.

Do not commit real shop photos, employee photos or private maps to this public repository. The photo-share handoff and the `wall-mapping-request` file from earlier versions were removed.
