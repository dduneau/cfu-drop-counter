# CFU Drop Counter

**Version 1.0**. The version is shown next to the app's name and written into the progress files and the Load settings record.

CFU Drop Counter counts bacterial colonies (CFU) in drops spotted on a 96-well plate lid filled with agar, and estimates the bacterial load of each fly from those counts. It runs in a web browser on an iPad, a phone or a computer, with nothing to install. Scans and counts are never uploaded. They stay on the device that opens the app.

The work goes through six steps, shown at the top of the app:

1. **Setup**: load the scan and describe the plate.
2. **Grid**: place the drop grid on the scan.
3. **Tune**: adjust the automatic detection on a sample of drops, then run it on the plate.
4. **Count**: check and correct each drop by tapping.
5. **Export**: save the counts as a CSV file.
6. **Load**: estimate the load per fly from one or several plates and a layout file.

The **Method** tab explains the statistics and gives the R code of the estimator. **Help** shows this guide.

## Getting started

1. Open the app's address in Safari on the iPad.
2. Tap **Share → Add to Home Screen**, and from then on open the app from that icon. Safari can clear a website's stored data after 7 days without a visit. Apps opened from the Home Screen are exempt.
3. Put the scans where the iPad can reach them: Files, iCloud Drive or AirDrop. Use the original image files. Photos and messaging apps can resize images.

**Use example scan** on the Setup screen loads an example plate to try the app.

## 1. Setup

**Plates on this device** lists every plate you have started. Each entry shows how many drops you have checked, when the plate was last edited, and whether a CSV or a backup file has been saved. **Open** returns to a plate exactly where you left it. **×** deletes it after you confirm. **New plate** starts an empty one.

**Scan.** Choose the plate image (PNG or JPEG; TIFF opens in Safari only). Choosing a scan for a plate you have already worked on starts a new plate, and the old one stays in the list.

**Plate information.** These fields become the first columns of the counts file, with one value for the whole plate. Fields can be text, whole numbers, decimals or yes/no, and you can add or remove them. Two field names have a special role:

- **Plate name** identifies the plate. The Load step uses it to match plates with the layout file. Give every plate its own name with a single number in it, such as `plate01`. Avoid names like `25-06-17_plate01`, because plates can be matched on the number in the name.
- **Replicate** appears next to the plate name in the lists, and goes up by one when you count another plate on the same scan.

**Colony types.** One row per kind of colony to count, each with a colour and an optional keyboard key. The default is a single type, Prett.

**Plate format.** 8 rows × 12 columns for a 96-well lid. It is locked as soon as the plate has counts, because those counts only make sense in that format. To change it, use **Clear all counts of this plate**, which can be undone on the Count screen.

**Volume and dilution** (folded away, because counting does not use them). The volume per drop and the dilution of each well are written into the counts file, and the Load step uses them.

- The dilution can be one value for the whole plate, one per row, or one per column.
- Write dilutions as factors: 1 = undiluted, 10 = 1/10, 25 = 1/25.
- **Leave a box empty for a row or column that was not spotted.** Those wells get `NA` as their dilution and are left out of the load estimate.

The app remembers these settings for the next plate.

## 2. Grid

1. Drag the four corner markers onto the centres of the corner drops: **A1, A12, H1 and H12**. A magnifier shows the spot under your finger. On a scan holding two plates, **Zoom to grid** enlarges the plate you are placing.
2. Tap **Snap to wells**. It fits the grid to the well rims when each circle is already within about a fifth of a well of its place. **Undo** puts the corners back.

A plate scanned sideways works too: put each marker on the well with that name. For a lid photographed with the dilution series running down the image, A1 is top left, A12 bottom left, H1 top right and H12 bottom right.

**Next: tune the detection** opens the Tune step.

## 3. Tune

Tune shows eight drops picked across the plate, from empty to crowded, with the colonies the detection finds in each. Every change of a setting updates them at once, so you can judge a setting on several kinds of drop together. **Show other drops** picks another eight. Tap a drop to open it in Count.

- **Image used by the detection**: brightness, contrast and sharpness of the image the detection works on. They help most on dim or low-contrast images. On a sharp, well-lit scan they make little difference.
- **Counting area**: the circle in which colonies are detected. Keep it just inside the well rim.
- **Sensitivity**: higher finds fainter colonies; lower ignores faint spots.
- **Colony size**: the expected size of one colony, which decides how merged colonies are split.

These settings are saved with the plate, and the next plate starts from the same ones.

**Detect on all drops** runs the detection on the whole plate and opens Count. **Count by hand, no detection** skips it.

**Coming back to Tune later is safe.** Once the plate has counts, the main button becomes **Detect again on the drops not checked yet**: drops you have looked at or corrected are never touched. **Detect again on every drop…** replaces those as well, after a confirmation. Either run can be undone with **Undo** on the Count screen.

## 4. Count

The drop fills the screen, with the detected colonies circled.

- **Tap an empty spot** to add a colony of the selected type.
- **Tap a mark** to remove it. With several colony types, switch **Tap a mark to** to **Change type** to give it the selected type instead.
- **Too many** next to a type means the drop cannot be counted for that type. It is saved as `Inf`.
- **Empty drop** sets the count to 0.
- **Ignore drop** leaves the drop out; it is saved as `NA`.
- **Undo** and **Redo** step back and forward through your changes: added or removed colonies, changed types, the drop flags, Clear marks and re-detections. Each plate keeps its own history of the last 100 steps, even after closing the app. A new change after an undo starts a new branch, so the steps you undid can no longer be redone.
- Move between drops with **Previous / Next**, by swiping left or right on the drop, or with the plate map, where each well shows its count. On the map, shaded wells were detected but not yet looked at, and wells outlined in green were corrected by hand. **Jump to next unchecked drop** finds the next drop you have not looked at.
- **Hold to hide marks** (bottom left of the drop) shows the drop without any circles for as long as you hold it, so you can check that no colony is hidden under a mark. On a keyboard, hold H.
- **Detect again on this drop** reruns the detection on the drop shown, and on no other. Its **Sensitivity** and **Colony size** sliders start from the plate's Tune settings on every drop, so they only rescue a difficult drop. To change the settings for the whole plate, go back to Tune.
- **View: Brightness, Contrast and Sharpness** change only what you see on this screen, to check a difficult drop. They do not affect the detection.

With a keyboard:

- ← → or space move between drops, and ↑ ↓ move by a row.
- The type keys select a colony type, and holding H hides the marks.
- Cmd/Ctrl + Z undoes, and Cmd/Ctrl + Shift + Z (or Ctrl + Y) redoes.

**How well detection works.** On the lab's scans, about 85% of countable drops came within ±2 colonies of a manual count, with a mean error near one colony. Detection misses colonies in crowded drops where they merge, and occasionally splits a large colony in two. It is a starting point, not a count: look at every drop before exporting.

## 5. Export

The Export screen shows a summary of the plate and a table of counts per drop. Shaded cells were detected but not looked at.

**Save CSV** writes the counts file, named after the scan: `COUNTS_<scan name>.csv`.

- When several plates come from the same scan, the plate name is added: `COUNTS_<scan>_<plate>.csv`.
- If those plates also share a name, the replicate is added as well.
- You can edit the name before saving.
- On an iPad the file goes to Files → Downloads.

**Save progress file** writes a backup of the plate (see *Saving and backups*).

**Count another plate on this scan** starts the next plate on the same image, keeping the grid so you only need to move it. **New plate with another scan** starts a fresh plate. Both keep the current plate in the list.

### The counts file

The file is separated by semicolons, with one row per well:

```
Date;Replicate;Plate name;WELL;Volume;Dilution;Prett;NA
25-06-17;1;plate01;A1;5.0;1.0;2;0
```

| Column | Content |
|---|---|
| Plate information fields | One value for the whole plate. |
| WELL | A1 … H12. |
| Volume | µL per drop. |
| Dilution | The dilution factor of that well, or `NA` when the well was not spotted. |
| One column per colony type | The count, `Inf` (too many to count), `0` (empty drop) or `NA` (ignored drop). |
| NA | Colonies without a type. Always 0, because every mark has a type. The column keeps the file in the lab's established counts format. |

## 6. Load

The Load step estimates the bacterial load of each fly from all of that fly's spots, by maximum likelihood. Open it after counting. The layout is only read here, so you stay blind to the treatments while you count.

**Plates.** Tick the plates on this device to include, and add **counts files** for plates counted on another device or earlier. Each plate brings its own volumes and dilutions. That means plates diluted per row, per column or as a whole plate can be combined, so one table can cover a whole experiment.

**Layout.** A file with one row per fly, giving an **ID** and where the fly sits:

| Column | Meaning |
|---|---|
| `ROW` | The fly occupies every well of that row (A–H); its dilution series runs across the columns. |
| `COL` | The fly occupies every well of that column (1–12). |
| `WELL` | The fly occupies one well (A1 … H12). Use one row per well. |
| `Plate` | Optional. Needed when different plates hold different flies. Without it, the same flies sit in the same wells on every plate. |
| Any other column | Copied into the result (sex, line, age, time after infection …). |

```
ROW;ID;Sex;Line;Time_post_injection
A;F01;Female;CantonS;24h
B;F02;Male;CantonS;24h
```

The file can be separated by semicolons, commas or tabs. When plate names are spelled differently in the layout and the counts (`plate01` and `plate1`), the app matches them on the number in the name and says so.

### Settings

- **Colony type** to estimate, or all types added together.
- **Spots with more colonies than can be read.** Counts stop being reliable once colonies merge, so spots above a **ceiling** (default 30) need special handling. There are three options, and they are different estimators rather than stricter or looser versions of one:
  - *Keep them as counts known to exceed the ceiling* (default). Each such spot contributes only the fact that it held more than 30 colonies. This is a censored Poisson model, and it uses every spot.
  - *Leave them out, and correct the rest for the ceiling*. Counts at or below the ceiling are modelled as Poisson counts known not to exceed it. This is a truncated Poisson model.
  - *No ceiling*. Every finite count is used as it stands.
- **Homogenate**: µL per fly (default 250), used to scale the load to a whole fly.
- **Log base**: log10 or log2. It rescales the results and changes no fit.
- **Dilution twice**: shown only when a fly has the same dilution more than once. Choose whether to use both copies or only the first.

**Why spots above the ceiling are kept by default.** The two models were compared on simulated flies of known load, using the lab's design: dilutions 1, 10, 100, 5, 25, 125, 3, 9, 27, 81, 5 µL drops and a ceiling of 30. Keeping the spots gave estimates as precise or more precise at every load. At high loads, where only one spot falls under the ceiling, leaving spots out produced wild overestimates and keeping them did not. The weakness of keeping them is that it trusts counts just below the ceiling, and a ceiling of 30 limits that.

### Checks

The app checks the data before estimating anything.

- These **stop the estimate**:
  - the same spot recorded twice (usually the same plate added twice);
  - a well holding two flies in the layout;
  - no plate matching the layout.
- These are **warnings**:
  - wells with colonies but no fly;
  - plates with no fly from the layout;
  - layout plates with no counts;
  - dilution factors between 0 and 1, which look like fractions typed by mistake;
  - drops not yet looked at.

Tables list the plates, the counts per dilution and the spots left out.

### The per-fly table

**Save per-fly table** writes `load_per_fly_<date>.csv`. **Save settings and checks** writes a text record of every setting and check, for the methods section.

| Column | Meaning |
|---|---|
| ID and layout columns | As in the layout file. |
| Plates | The plates that contributed spots. |
| Spots, Countable, Above_ceiling, Too_many | How the fly's spots were classified. |
| Total_CFU | All finite colonies counted for that fly. |
| LOD_per_fly | Detection limit: one colony over all the volume plated for that fly, scaled to the homogenate. |
| load, logload, se.logload | Load per µL of homogenate, its log, and the standard error of the log. |
| sumCFU, code | Colonies used by the estimator. code 1 = estimated, −1 = no estimate. |
| CFU_per_fly, log10_CFU_per_fly (or log2), se_log | Load per fly, and its log with standard error. |
| Status | *Detected*, *Not detected*, *Above the dilutions used* or *Estimation failed* (see below). |

The statuses mean:

- **Detected**: a load was estimated.
- **Not detected**: every spot was countable and empty.
- **Above the dilutions used**: no spot was countable, so the load is higher than this series can measure.
- **Estimation failed**: none of the above applies.

The detection limit is the load a single colony implies, not the load at which detection becomes reliable.

#### How the estimate is made

Each spot's colony count is modelled as Poisson, with mean equal to the fly's load per µL multiplied by the spot's volume and dilution. The load is estimated by maximum likelihood over all of the fly's spots. With the default setting, spots above the ceiling make it a censored Poisson model. With *Leave them out*, it is a right-truncated Poisson model.

The **Method** tab gives the full description: the likelihood for each setting, the standard error, the detection limit and the assumptions. It also shows the R code of the estimator, the lab's `loadEstimate()`, which the app runs translated into JavaScript, and an R example that reproduces the per-fly table from a counts file and a layout file. The translation agrees with the R code to about 10⁻¹².

## Saving and backups

**Everything is saved automatically**, at every tap, in the browser's storage on the device. This storage is not a file you can see in Files. The app reads it by itself, so when you reopen the app you are back where you stopped.

**A web page cannot write files by itself on an iPad.** Every file it saves needs you to confirm it, goes to Downloads, and cannot be updated or deleted by the app afterwards.

**The progress file** (`PROGRESS_<scan name>.json`) is a backup you save by hand. It holds the marks, flags, grid position and plate settings, but not the image. Save one from time to time, and keep it next to the scan.

**To continue a plate on another device**, or after the iPad's data was lost:

1. Copy the progress file and the original scan to that device.
2. Open the app and go to **Setup**.
3. Tap **Open progress file…** and choose the scan, in either order.

The app checks the scan's pixel size, so the marks cannot land on the wrong image. The two devices do not stay in sync, so keep working on one of them. The layout file and the Load settings are not in the progress file.

## Hosting and updating

The app is a single self-contained file, `index.html`, with no server code. It is hosted on GitHub Pages from this repository.

- **To update the app**, replace `index.html`. Plates saved on the iPad are kept.
- **Saved plates belong to the web address.** If the app moves to another address, finish and export the plates first.
- **Fonts** load from Google Fonts. Without internet the app still works, in the device's standard fonts.

## Source code

`index.html` is assembled from the files in `src/`. See `src/DEVELOPING.md` for how to build it and run the tests. Change the source rather than editing `index.html` directly, so that the tests and the estimator checks can be rerun.

## Limits

- The Load step has no plot. The per-fly table is meant for analysis in R or another tool.
- On a phone the drop view is small, so crowded drops are harder to correct. Use an iPad for full plates.
