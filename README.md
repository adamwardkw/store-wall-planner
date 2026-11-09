# Photo wall planner

Open https://adamwardkw.github.io/store-wall-planner/.

A single-file personal planning tool. No backend, API key, paid API, or employee account. The app code is public; imported photos and layouts are not uploaded to the repository.

## The flow

1. **Take a picture** opens the rear-camera chooser on supported phones. **Choose wall photo** also accepts an existing image. Photos are resized locally to at most 1600 × 1400 pixels.
2. **Send for mapping** opens the device share sheet with the photo, a mapping prompt, and an upload request file when the device supports sharing those file types. Choose the intended conversation yourself. This is a handoff, not an automatic upload. The app cannot confirm message delivery.
3. If sharing is unavailable, **Download upload file** creates `wall-mapping-request.json`, containing the photo and the ready-to-go prompt. Send that file to the person handling mapping. Employees use the same page and send their own files; there is no shared folder exposing everyone's images.
4. The mapping helper puts the photo and prompt into the owner's personal ChatGPT conversation and returns `wall-spot-map.json`. This is a human-assisted process, not a live ChatGPT integration. Employees do not get access to the owner's ChatGPT account.
5. **Import map** loads the returned JSON. If it has `photo: null`, the currently open photo is kept. Check that it is the same photo used for mapping. AI counts and coordinates must be visually checked. Manual **Mark spots** is available as a backup.
6. Enter brand, item label, colour, and count, then **Make line**. Select the line and tap its first mapped spot, or drag it onto the wall. Ten products become five, a one-position gap, then five: 11 physical positions. Distinct lines also need one empty position between them. A line that does not fit or overlaps an existing line is not placed.
7. **Clean model** removes the photo, leaving the mapped positions and products. Zoom to 2× or 3× to select dense positions on a phone, then pan inside the wall view. The placement table remains readable below it.
8. **Print / PDF** prints the current wall view and the placement plan. **Export plan** saves a full backup including the photo, spots, and products.

The app saves the open photo plan in local browser storage. Storage failure is shown with an export warning. Clearing browser data removes that device's saved plan. Keep an exported backup. A downloaded `index.html` works offline; the hosted page needs to have loaded before it can be used without a connection.

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
- `lines` may be empty for a new map. A populated line has `id`, `brand`, `item`, `count` (1-100), `color` (hex), `shelfId`, and zero-based `start`. Unplaced lines use `shelfId: null, start: null`. The app calculates five-unit runs and reserved gaps from those fields. Imports with overlapping runs are rejected.

## Upload request format

`wall-mapping-request.json` is a handoff envelope, not a placement map:

```json
{
  "format": "wall-mapping-request",
  "version": 1,
  "name": "Shop wall",
  "photo": {"dataUrl": "data:image/jpeg;base64,...", "width": 1536, "height": 1152},
  "prompt": "The ready-to-go mapping instructions..."
}
```

The mapping helper extracts the photo and prompt, uses the owner's chosen ChatGPT conversation, and returns a map in the first format. Do not commit real shop photos, employee uploads, or private maps to this public repository. There is no background upload listener, shared inbox, or automatic return-to-device route in this static page.
