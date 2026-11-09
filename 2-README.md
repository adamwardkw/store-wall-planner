# Wall Planner

A small internal tool for planning a store display wall. Set the wall's real
size, add your items, drag them into place, see what the finished wall will
look like, and print or save a to-scale map as a PDF before you touch the
actual wall.

## How to open it

Open the link and start planning:

**https://adamwardkw.github.io/store-wall-planner/**

No install, no accounts. It works on phone and desktop, and works offline
once loaded. Everything you make is saved automatically in the browser on
that device. Use **Export file** to download a backup or move a plan to
another device, and **Import** to load it back.

## What it does

- **To-scale wall**: set the wall in feet and inches; the canvas, grid
  (1 ft lines) and rulers (every 2 ft) stay true to scale.
- **True-to-life scene**: the wall renders as a real room - painted wall,
  baseboard and floor, with soft shadows and depth on every item.
- **Item library**: add anything that goes on the wall with a name, width,
  height, colour or photo, and a quantity. Press **Place** to put it on the
  wall. The library tracks how many are placed versus how many you have.
- **Plug and play**: drag items anywhere. Click one for the inspector, where
  you can type exact distances from the left edge and top, change its size,
  duplicate it, lock it, or remove it.
- **Preview**: one click hides the grid, rulers, numbers and labels so you
  see what the finished wall will actually look like.
- **Overlap warning**: items that collide are outlined in red so nothing gets
  double-booked.
- **Layouts**: save as many named arrangements as you like and flip between
  them. Use **Save as** to keep a variation before you experiment.
- **Undo / redo**: Ctrl+Z and Ctrl+Y, across every change.
- **Print / PDF**: one click produces a clean plan sheet: the to-scale wall
  with numbered items plus a legend listing each item's size, quantity and
  exact position. Choose "Save as PDF" in the print dialog to share it.

## Notes

- Single file: the whole app is `index.html`. No build, no server, no
  dependencies.
- Your plans never leave your device (browser localStorage only).
