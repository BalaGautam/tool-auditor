# Tool Auditor

Manage tool inventory: eliminate strays and foreign objects, and track missing and incorrect tools. Edge AI that runs in your browser, so your data never leaves for the cloud. Take a photo of a shadow board and the app checks it against the board's planogram, gives a compliance score, writes restock lines and keeps an audit log.

It runs entirely in the browser with [EmbeddingGemma 2](https://blog.google/innovation-and-ai/technology/developers-tools/embeddinggemma-2/) and Transformers.js. Photos are never uploaded.

## What it does

| Shadow board | Planogram term | In the app |
|---|---|---|
| Traced outline | Slot (A1, A2, B1…) | Auto-numbered by position, with name, part no. and unit cost |
| Tool gone | Out of stock / gap | **Missing** → restock line |
| Tool in another tool's outline | Misplaced | **Misplaced** → "move back to B2" |
| Something else in an outline | Foreign item | **Unknown item**, named by matching the photo to a list of item names |
| Something left on the board | Foreign object debris (FOD) | **Stray object**, found by comparing the board outside the slots |
| — | Compliance % | Slots holding their own tool ÷ (slots + stray objects). A stray pulls the score down: 9 tools present + 1 stray pencil = 90% |
| Tool doesn't fill its outline | Wrong size / swapped | **Outline check**: the coloured fill showing around a tool flags look-alike swaps (e.g. two spanners) that the image match alone can miss. Needs the empty-board photo. |

## Build the board

1. Glue white chart paper flat onto cardboard (glue stick or double-sided tape over the whole sheet). Lay the board on the floor.
2. Keep the whole white sheet in every photo with the cardboard showing around it. The app finds the paper's corners and straightens any photo. (On a light floor or table, draw a **solid black square, about 3 cm**, at each corner instead, 2–3 cm in from the edges; the app looks for those first.)
3. Trace each tool with a thick marker and fill the shape with a bright color. Leave a finger's width between tools.
4. In the app, mark the slots and press **Auto-number**. Rows are lettered from the top, or from the bottom if you tick *Letter rows from the bottom*. Write each code and name about 1 cm below its outline, e.g. "B1 · Hammer · $2.49".
5. Take photos standing over the board, with the whole sheet in frame, in even light and without flash.

## Inputs

JPEG, PNG, WebP, AVIF, GIF, BMP, **iPhone HEIC/HEIF** (converted in the browser), screenshots by paste (Ctrl/Cmd+V), drag and drop, the live camera, and **video clips** (MP4, MOV, WebM: scrub to a frame and use it).

## Run it

**Online:** https://balagautam.github.io/tool-auditor/

**On a laptop (Chrome or Edge), from a copy of this repo**
```bash
python3 -m http.server 8777
```
Open http://localhost:8777, then press **Load model**. The first load downloads about 285 MB, which the browser caches.

**On an iPhone:** the camera needs HTTPS, so use the GitHub Pages link once the repo is published (see below). You can also check iPhone photos on the laptop by AirDrop or by dropping the HEIC file in.

## Try it with the sample board

`test-photos/real/` holds a real board (photos from the author's garage): the empty and full baselines, plus checks for a spanner swap, a swap with the hammer missing, a stray pencil, a stray pencil with the hammer missing, and a smaller spanner in the wrong outline. In step 2 press **Import planogram** and pick `garage-shadow-board-planogram.json`; it brings in the slots and both baseline photos. Then load a check photo in step 3 and run it.

## Files and data

- `index.html` is the whole app.
- The planogram, the reference photos (straightened) and the audit log are stored in this browser only. Use **Export planogram** to back them up or move them to another device.
- Exports: planogram `.json` (with photos) and `.csv` spec, restock `.csv`, audit log `.csv`, annotated check photo `.jpg`.

## Limits

- The "Looks like" name is the model's best guess from the item-name list, not a certainty.
- Without the empty-board photo, the app can't tell an unknown item from a missing one.
- The stray scan compares colour away from where the tools lie, so a big lighting change or a strong shadow can raise a false alarm. Re-take the photo or turn the scan off.
- A bigger tool placed alone in a smaller outline covers the coloured fill, so only the image match can catch it. In a swap the other slot gives it away.
- A check takes about 75 seconds on a laptop GPU at Standard detail; Low detail is faster.

## License

MIT for this app. EmbeddingGemma 2 is Apache 2.0.
