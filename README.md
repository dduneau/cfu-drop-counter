# CFU Drop Counter

**Version 1.0**. The version is shown next to the app's name, and written into the progress files and the Load settings record.

A web app for counting bacterial colonies (CFU) in drops spotted on a 96-well plate lid filled with agar, and for turning those counts into a bacterial load per fly. It runs in the browser, on an iPad, a phone or a computer. Nothing is uploaded: scans and counts stay on the device that opens the app.

It replaces the Java program CountDrops for counting. Its COUNTS files have the same format, and its Load tab uses the same estimator as the lab's R app, `loadEstimate()`.

## Getting started

1. Open the app's address in Safari on the iPad.
2. Tap **Share → Add to Home Screen**, and from then on open the app from that icon. Safari can clear a website's stored data after 7 days without a visit. Apps opened from the Home Screen are exempt.
3. Put the scans where the iPad can reach them: Files, iCloud Drive or AirDrop. Use the original image files. Photos and messaging apps can resize images.

The app works through five steps, shown at the top: **Setup → Grid → Count → Export → Load**.

## 1. Setup

**Plates on this device** lists every plate you have started, with how many drops you have checked, when it was last edited, and whether a CSV or a backup file has been saved. **Open** switches to a plate exactly where you left it. **×** deletes it, after you confirm. **New plate** starts an empty one.

**Scan.** Choose the plate image (PNG or JPEG; TIFF opens in Safari only). Choosing a scan for a plate you have already worked on starts a new plate, and the old one stays in the list. **Use example scan** loads a reduced example to try the app.

**Plate information.** These fields become the first columns of the COUNTS file, one value for the whole plate. Text, integer, decimal and yes/no fields can be added or removed. Two field names have a meaning:

- **Plate name** names the plate. The Load tab and the R app use it to match plates with the layout file. Give every plate its own name, with a single number in it (`plate01`, not `25-06-17_plate01`), because plates can be matched on the number in the name.
- **Replicate** is added to the plate's name in the lists, and goes up by one when you count another plate on the same scan.

**Colony types.** One row per kind of colony to count, each with a colour and an optional keyboard key.

**Plate format.** 8 × 12 for a 96-well lid.

**Volume and dilution** (folded away; not used for counting). The volume per drop and the dilution of each well are written into the COUNTS file, and the Load tab uses them. The dilution can be one value for the whole plate, one per row, or one per column. Dilutions are written as factors: 1 = undiluted, 10 = 1/10, 25 = 1/25. **Leave a box empty for a row or column that was not spotted.** Its wells are exported with `NA` as dilution and left out of the load estimate. The app remembers these settings for the next plate.

## 2. Grid

Drag the four corner markers onto the centres of the corner drops: **A1, A12, H1 and H12**. A magnifier shows the spot under your finger, and **Zoom to grid** enlarges the plate on a scan holding two plates. Then tap **Snap to wells**: it fits the grid to the well rims when each circle is already within about a fifth of a well of its place, and **Undo** puts the corners back.

A plate scanned sideways works too: put each marker on the well that carries that name. For a lid photographed with the dilution series running down the image, A1 is top left, A12 bottom left, H1 top right and H12 bottom right.

**Counting area** sets the size of the circle in which colonies are detected; keep it just inside the well rim.

**Detect colonies on all drops** runs the automatic detection on every drop and opens the counting screen. **Count by hand** skips detection.

## 3. Count

The drop fills the screen, with the detected colonies circled.

- **Tap an empty spot** to add a colony of the selected type.
- **Tap a mark** to remove it. Switch **Tap a mark to** to **Change type** to give it the selected type instead.
- **Too many** next to a type means the drop cannot be counted for that type. It is exported as `Inf`.
- **Empty drop** sets the count to 0. **Ignore drop** leaves the drop out, and it is exported as `NA`.
- **Undo** reverses the last change, including a whole re-detection.
- Move with **Previous / Next**, by swiping left or right on the drop, or with the plate map, where each well shows its count.
  - On the map, shaded wells were detected but not yet looked at, and green-outlined wells were corrected by hand.
  - **Jump to next unchecked drop** finds the next one you haven't looked at.
- **Detection sensitivity** and **Colony size** adjust the detection. **Detect again on** reruns it on this drop, its row, its column or the whole plate.

With a keyboard: ← → or space move between drops, ↑ ↓ move by a row, the type keys select a type, and Cmd/Ctrl + Z undoes.

**How well detection works.** On the lab's scans it gets about 85% of countable drops within ±2 colonies of a manual count, with a mean error near one colony. It tends to miss colonies in crowded drops where they merge, and occasionally splits a large colony in two. Look at every drop before exporting. Detection is a starting point, not a count.

## 4. Export

A summary of the plate and a table of counts per drop. Shaded cells were detected but not looked at.

**Save CSV** writes the COUNTS file, named after the scan: `COUNTS_<scan name>.csv`. When several plates come from the same scan, the plate name is added (`COUNTS_<scan>_<plate>.csv`), and the replicate too if the plate names are the same. The name can be edited before saving. On an iPad the file goes to Files → Downloads.

**Save progress file** writes a backup of the plate (see *Saving and backups* below).

**Count another plate on this scan** starts the next plate on the same image, with the grid kept so you only move it. **New plate with another scan** starts a fresh plate. Both keep the current plate in the list.

### The COUNTS file

Semicolon-separated, one row per well, the same format as CountDrops:

```
Date;Replicate;Plate name;WELL;Volume;Dilution;Prett;Unknown;NA
25/06/2017;1;Traj_Prett_CS_sexdim_plate01;A1;5.0;25.0;2;0;0
```

| Column | Content |
|---|---|
| Plate information fields | One value for the whole plate. |
| WELL | A1 … H12. |
| Volume | µL per drop. |
| Dilution | The dilution factor of that well, or `NA` when the well was not spotted. |
| One column per colony type | The count, `Inf` (too many to count), `0` (empty drop) or `NA` (ignored drop). |
| NA | Colonies with no type. Always 0 in this app. Kept for compatibility. |

## 5. Load

Estimates the bacterial load per fly from every spot of each fly, by maximum likelihood. Open it after counting: the layout is only read here, so you stay blind to the treatments while you count.

**Plates.** Tick the plates on this device to include, and add **COUNTS files** for plates counted elsewhere or earlier. Each plate brings its own volumes and dilutions. Plates diluted per row, per column or as a whole plate can therefore be combined, so one table can cover a whole experiment.

**Layout.** A file with one row per fly: an **ID** column, and where the fly sits.

| Column | Meaning |
|---|---|
| `ROW` | The fly occupies every well of that row (A–H): its dilution series runs across the columns. |
| `COL` | The fly occupies every well of that column (1–12). |
| `WELL` | The fly occupies one well (A1 … H12). Use one row per well. |
| `Plate` | Optional. Needed when different plates hold different flies; without it, the same flies sit in the same wells on every plate. |
| Any other column | Copied into the result (sex, line, age, time after infection …). |

```
ROW;ID;Sex;Line;Time_post_injection
A;F01;Female;CantonS;24h
B;F02;Male;CantonS;24h
```

Semicolons, commas or tabs all work. When the plate names in the layout and the counts are spelled differently (`plate01` and `plate1`), the app matches them on the number in the name and says so.

**Estimation settings**

- **Colony type** to estimate, or all types added together.
- **Spots with more colonies than can be read.** Spots above the **ceiling** (default 50) can be handled three ways. These are three different estimators, not one strict option and one loose one.
  - *Leave them out and correct the rest for the ceiling*: a truncated Poisson.
  - *Keep them as counts known to exceed the ceiling*: a censored Poisson, which uses every spot.
  - *No ceiling*: every finite count is used as it stands.
- **Homogenate**: µL per fly (default 250), used to scale the load to a whole fly.
- **Log base**: log10 or log2. It rescales the numbers and changes no fit.
- **Dilution twice**: only shown when a fly has the same dilution more than once. Choose whether to use both copies or the first only.

**Checks** run before anything is estimated.

- These **block the estimate**:
  - the same spot recorded twice (usually the same plate added twice);
  - a well holding two flies in the layout;
  - no plate matching the layout.
- These are **warnings**:
  - wells with colonies but no fly;
  - plates with no fly from the layout;
  - layout plates with no counts;
  - dilution factors between 0 and 1, which look like fractions typed by mistake;
  - drops not yet looked at.
- Tables list the plates, the counts per dilution and the spots left out.

### The per-fly table

**Save per-fly table** writes `load_per_fly_<date>.csv` with the same columns as the R app, plus `Plates`. **Save settings and checks** writes a text record for the methods section.

| Column | Meaning |
|---|---|
| ID and layout columns | As in the layout file. |
| Plates | The plates that contributed spots. |
| Spots, Countable, Above_ceiling, Too_many | How the fly's spots were classified. |
| Total_CFU | All finite colonies counted for that fly. |
| LOD_per_fly | Detection limit: one colony over all the volume plated for that fly, scaled to the homogenate. |
| load, logload, se.logload | Load per µL of homogenate, its log, and the standard error of the log. |
| sumCFU, code | Colonies used by the estimator; 1 = estimated, −1 = no estimate. |
| CFU_per_fly, log10_CFU_per_fly (or log2), se_log | Load per fly and its log with standard error. |
| Status | *Detected*. *Not detected*: every spot countable and empty. *Above the dilutions used*: no countable spot. *Estimation failed*. |

**About the estimator.** `loadEstimateOneSample()` and `loadEstimate()` were rewritten line by line in JavaScript. That includes R's root finder (`uniroot` with `extendInt = "downX"`) and R's method for Poisson probabilities. The rewrite was compared with the original R function, run in R 4.6.

- **1,415 simulated samples** under every ceiling setting, plus edge cases. The estimates agree to about 10⁻¹², with the same successes and failures.
- **The lab's 48-fly, 8-plate example.** Counts, statuses and detection limits are identical, and the loads agree to 10⁻¹³.
- In the "leave them out" setting the standard error can differ from R in the ninth significant digit, which is far below anything reported.

## Saving and backups

**Everything is saved automatically**, at every tap, in the browser's storage on this device. That storage is not a file you can see in Files. The app reads it by itself: reopen the app and you are back where you stopped.

**A web page cannot write files by itself on an iPad.** Every saved file needs you to confirm it, goes to Downloads, and cannot be updated or deleted by the app afterwards.

**The progress file** (`PROGRESS_<scan name>.json`) is a backup you save by hand. It holds the marks, flags, grid position and plate settings, but not the image. Save one now and then, and move it next to the scan if you like.

**To continue a plate on another device** (or after the iPad's data was lost):

1. Copy the progress file and the original scan to that device.
2. Open the app and go to **Setup**.
3. Tap **Open progress file…** and choose the scan. Either order works. The app checks the scan's pixel size so the marks cannot land on the wrong image.

The two devices do not stay in sync, so keep working on one. The layout file and the Load settings are not in the progress file.

## Hosting

The app is one self-contained file, `index.html`, with no server code.

- It is hosted on GitHub Pages from this repository.
- To update it, upload a new `index.html` over the old one. Plates saved on the iPad are kept.
- Saved plates belong to the web address. If the app moves to another address, finish and export the plates first.
- The page loads its fonts from Google Fonts. Without internet it still works, in the device's standard fonts.

## Limits

- No plot in the Load tab. The per-fly table goes to the downstream R analyses.
- On a phone the drop view is small, so crowded drops are harder to correct. Use an iPad for full plates.
