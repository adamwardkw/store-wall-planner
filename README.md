# Wall Planner

A small internal tool for planning a store display wall. Two ways to plan:

- **Plug & play** (the default): no measuring. Add a row, say how many spots
  it has, tap each spot to drop an item in, and move on. The wall is just
  rows of spots, counted in items, not feet.
- **Measured**: the original to-scale canvas. Set the wall's real size, add
  items with real dimensions, drag them into place, and print a to-scale map.

## How to open it

Open the link and start planning:

**https://adamwardkw.github.io/store-wall-planner/**

No install, no accounts. It works on phone and desktop, and works offline
once loaded. Everything you make is saved automatically in the browser on
that device. Use **Export file** to download a backup or move a plan to
another device, and **Import** to load it back.

## Plug & play mode

- **Rows of spots**: press **Add a row** (or pick a starter like "3 rows of
  5"). Rows stack top to bottom; each row fills the wall edge to edge.
- **Tap to fill**: tap a spot, pick an item from the list, or type a new
  name to create it on the spot. The picker moves to the next empty spot
  automatically, so filling a row is tap after tap.
- **Row controls**: each row has small buttons to add or remove a spot,
  move it up or down, duplicate it, or delete it.
- **Labels**: the item's name is the label - no extra step.
- **Counts**: the library and the line under the wall show how many of each
  item are on the wall.
- **Preview**: one click hides the spots, numbers and controls so you see
  what the finished wall will actually look like.
- **Print / PDF**: a clean plan sheet with the spot map, a spot-by-spot
  legend (spot 1.3 = row 1, third from the left), and totals per item.
  Handy as a restocking list.

## Measured mode

- **To-scale wall**: set the wall in feet and inches; the canvas, grid
  (1 ft lines) and rulers (every 2 ft) stay true to scale.
- **Item library**: add anything with a name, width, height, colour or
  photo, and a quantity. Press **Place** to put it on the wall.
- **Precise placement**: drag items anywhere. Click one for the inspector:
  exact distances from the left edge and top, size, duplicate, lock, remove.
- **Overlap warning**: items that collide are outlined in red.
- **Print / PDF**: the to-scale wall with numbered items plus a legend with
  sizes, quantities and exact positions.

## Both modes

- **True-to-life scene**: painted wall, baseboard and floor, with soft
  shadows and depth on every item.
- **Layouts**: save as many named arrangements as you like and flip between
  them. Each mode keeps its own layouts.
- **Undo / redo**: Ctrl+Z and Ctrl+Y, across every change.

## Notes

- Single file: the whole app is `index.html`. No build, no server, no
  dependencies.
- Your plans never leave your device (browser localStorage only).
