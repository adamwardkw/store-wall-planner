# Wall Planner

A small internal tool for planning a store display wall. Set the wall's real
size, add your items, drag them into place, and print or save a to-scale map
as a PDF before you touch the actual wall.

## How to open it

No install, no server, no accounts. Download or clone this folder and
double-click `index.html`. It opens in any modern browser and works offline.

Everything you make is saved automatically in the browser on that computer
(localStorage). Use **Export file** to download a backup or move a plan to
another computer, and **Import** to load it back.

## What it does

- **To-scale wall**: set the wall in feet and inches; the canvas, grid
  (1 ft lines) and rulers (every 2 ft) stay true to scale.
- **Item library**: add anything that goes on the wall with a name, width,
  height, colour or photo, and a quantity. Press **Place** to put it on the
  wall. The library tracks how many are placed versus how many you have.
- **Plug and play**: drag items anywhere. Click one for the inspector, where
  you can type exact distances from the left edge and top, change its size,
  duplicate it, lock it, or remove it.
- **Preview**: one click hides the grid, rulers and numbers so you see what
  the finished wall will actually look like.
- **Overlap warning**: items that collide are outlined in red so nothing gets
  double-booked.
- **Layouts**: save as many named arrangements as you like and flip between
  them. **Save as** copies the current one so you can try variations.
- **Undo / redo**: buttons in the header, or Ctrl+Z / Ctrl+Y.
- **Print / PDF**: produces a clean plan sheet with the wall drawn to scale,
  numbered items, and a legend listing each item's size, quantity, and exact
  position. Choose "Save as PDF" in the print dialog to share it.

## Typical flow

1. Measure the wall, enter the size up top.
2. Add each display item once (measure the real thing).
3. Place and drag until the wall looks right. Use Preview to sanity-check it.
4. Save the layout under a name, or Save as to try another arrangement.
5. Print / PDF the winner and bring it to the shop.

## Notes

- Single HTML file, no dependencies, no tracking, nothing leaves your browser.
- Designed with help from ChatGPT (design spec and UX review).
