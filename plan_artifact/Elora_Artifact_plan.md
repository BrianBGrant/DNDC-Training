# Elora model–data check: how it was built

A single web page that compares a DNDCv.CAN run of **Elora_P1_history** (1990–2023) with field measurements. It shows a scorecard of fit statistics, plots over time and a 1:1 scatter for each variable. A user can load their own run folder in the browser to replace the built-in sample run.

- **Live page:** https://claude.ai/artifact/8gM5h4V33ax2abM6gpR4Jz
- **Offline copy:** `elora_report.html` in this folder. It is one self-contained file that opens in any browser and needs internet only to load the chart library.
- **Source:** `elora_page_source.zip` in this folder (see section 8).

---

## 1. Goal

Workshop participants run DNDCv.CAN for Elora, then check how well the model matches measurements without writing any code. The page has to:

1. Work with no install: a static HTML page, with all files read in the browser and nothing uploaded.
2. Read DNDCv.CAN output files directly from the run folder.
3. Carry the Elora observations inside the page.
4. Give standard calibration statistics (RMSE, nRMSE, NARE, d, r²) for each variable and depth.
5. Open with a sample run already loaded, so the page is useful before anyone runs the model.

## 2. Architecture

```
template.html   page layout, styles, UI logic, charts (Chart.js 4.4.1 from jsDelivr)
core.js         parsing, unit conversion, pairing, aggregation, statistics
obs.json        Elora observations, pre-processed into compact arrays
sample.json     the default DNDC run, pre-processed into model-time arrays
assemble.js     inlines core.js, obs.json and sample.json into template.html
                → elora_report.html (the published page)
build.js        builds obs.json (and a sample) from the raw CSVs, and prints stats for checking
build_sample2.js  rebuilds sample.json only, from a new set of DNDC output files
```

`core.js` is shared by the page and by Node, so the same code that runs in the browser can be checked from the command line.

## 3. Inputs

### 3.1 DNDCv.CAN output files (the run folder)

Default run folder on the workshop PCs:
`C:\DNDC_WIN\output\Elora\Record\Batch\Case1-Elora_P1_history`

| File | Variables taken | Conversion |
|---|---|---|
| `Multi_year_summary.csv` | Grain C of the main crop each year (the crop slot with the largest grain C, out of up to 4 crops) | grain C ÷ grain C fraction (default 0.4) = kg DM/ha |
| `Day_SoilN_1.csv` | N₂O flux (`N2O-flux` column), NH₄⁺ + exchangeable NH₄⁺, NO₃⁻ | N₂O: kg N/ha/day × 1000 = g N/ha/day. 0–15 cm = 0–10 cm layer + half of 10–20 cm. 15–30 cm = half of 10–20 cm + 20–30 cm. |
| `Day_SoilClimate_1.csv` | Soil temperature and WFPS by depth, bulk density | Temperature: 5 cm, and the mean of the two model depths around 25, 55 and 85 cm. Moisture: WFPS × (1 − bulk density / 2.65) = volumetric m³/m³. |
| `Day_SoilC_1.csv` | NEE (`NEE` column) | kg C/ha/day ÷ 10 = g C/m²/day |

**Model time.** DNDC years have 365 days. Each daily value is stored at index `(YearIndex − 1) × 365 + (Day − 1)` and mapped to a calendar date using the "First simulated year" setting (default 1990). Rows are recognised by an integer in the first column, which skips the multi-row headers.

### 3.2 Observations (built into the page)

| File | Columns used |
|---|---|
| `Elora_OBS_YIELD_DM.csv` | Year, Crop (CORN / SOY / WW), Yield_DM (kg DM/ha) |
| `Elora_OBS_N2O.csv` | daily N₂O, g N/ha/day |
| `Elora_OBS_NH4NO3.csv` | NH₄⁺ and NO₃⁻ at 0–15 and 15–30 cm, kg N/ha |
| `Elora_OBS_soilT.csv` | soil temperature at 5, 25, 55 and 85 cm |
| `Elora_OBS_VMC.csv` | volumetric moisture at 5, 25, 55 and 85 cm, m³/m³ |
| `Elora_OBS_CO2.csv` | obs_nee, g C/m²/day |

Dates are M/D/YYYY. `NaN`, `NA` and blank cells count as missing. `build.js` turns each column into a pair of arrays: day numbers since 1 Jan 1990, and values.

## 4. Matching model to measurements

1. **Pair by day.** A pair is made only on days that have a measurement *and* a model value, inside the "Compare years" range (default 2000–2023).
2. **Excluded years.** 2012 and 2013 are left out by default. The "Exclude years" box takes any list, such as `2012, 2013`.
3. **Soil moisture filters** (each can be switched off):
   - Measured values outside 0–0.7 m³/m³ are dropped.
   - **May 1 – Oct 31 only.** The statistics *and* the plots cover the growing season only, and the model line breaks over winter.
   - **Skip frozen soil.** Days when measured or simulated soil temperature at the same depth is ≤ 0 °C are dropped, because the sensors read only liquid water.
4. **Yield.** Each observed year is paired with the simulated main crop of that year. Separate rows show all crops, Corn, Soybean and Winter wheat.
5. **N₂O aggregation.** Each period is the mean of the paired days in it:
   - **Weekly:** Monday–Sunday, kept when a week has at least 4 measured days.
   - **Monthly:** calendar months, kept when a month has at least 10 measured days.
   - **Average month:** every January pooled across all selected years, then every February, and so on, giving 12 points. The model is averaged over the same days as the measurements, so gaps in the record affect both equally.
   - The annual bar chart sums only the days that have a measurement.

## 5. Statistics (paired values only)

With S = simulated, O = observed, n = number of pairs, and Ō = observed mean:

| Metric | Formula | Read it as |
|---|---|---|
| RMSE | √(Σ(S−O)² / n) | typical error, in the variable's units |
| nRMSE % | 100 × RMSE / \|Ō\| | error relative to the mean (Jamieson classes: <10 excellent, 10–20 good, 20–30 fair, >30 poor) |
| NARE % | 100 × (S̄ − Ō) / \|Ō\| | bias: + over-predicts, − under-predicts |
| d | 1 − Σ(S−O)² / Σ(\|S−Ō\| + \|O−Ō\|)² | Willmott index of agreement, 0–1 |
| r² | squared Pearson correlation | pattern only, blind to bias |

The scorecard colours d: green ≥ 0.90, yellow 0.70–0.89, red < 0.70. Statistics need at least 3 pairs.

nRMSE and NARE swing widely when the observed mean is near zero (NEE, deep NH₄⁺). Read RMSE and d for those variables.

## 6. Page layout

1. **Run panel.** "Choose run folder" reads the whole folder (`webkitdirectory`) and "Choose files" picks individual CSVs. Four file chips show where each file came from (sample run or your folder), and a "Copy path" button copies the workshop run folder path.
2. **Settings.** First simulated year, grain C fraction, compare years, exclude years, soil moisture season, and skip frozen soil. Every change recalculates the page instantly.
3. **Scorecard.** One row per variable and depth. Clicking a row opens it in the explorer.
4. **Explorer tabs.** Yield, N₂O, NEE, Soil N, Soil temperature, Soil moisture, each with a time series, a 1:1 scatter and the stats row. Yield and N₂O also get annual bar charts.
5. **"How the numbers are calculated".** The docs in sections 3–5 above, built into the page.

Files loaded from a folder stay in that viewer's browser only. A shared link always opens with the built-in sample run.

## 7. Updating the default run

1. Copy the four output files from the new run folder.
2. Point `build_sample2.js` at them and run `node build_sample2.js`. This rewrites `sample.json`.
3. Run `node assemble.js` to rebuild `elora_report.html`.
4. Check it: open `test.html` (the same page using a local copy of Chart.js), or run `node shot.js`, which prints the scorecard and checks the page for errors and horizontal scrolling at phone width.
5. Republish the page to the same link.

## 8. Rebuilding or adapting it for another site

**Requirements:** Node.js. Playwright is only needed for `shot.js`.

```
node build.js          # observations + sample → obs.json, sample.json (edit the file paths at the top first)
node assemble.js       # → elora_report.html and test.html
node shot.js           # optional screenshot / layout check
```

The Ottawa page (https://claude.ai/artifact/GA3EJm2ZbC2N8Fw7sssvSk) was made from this same code, with these changes:

- **Observations:** a different set of files. Soil N comes from the West location only, as the mean of replicates, with "<x" values set to x/2.
- **Depths:** soil N at 0–15, 15–30, 30–50 and 50–70 cm. Soil temperature and moisture at 5 and 10 cm.
- **Yield:** observations are at harvest moisture and are converted to dry matter, with an editable moisture for each crop (corn 15.5 %, wheat 14.5 %, canola 10 %, soybean 14 %). Crop IDs are 1 corn, 6 spring wheat, 57 canola and 3 soybean.
- **No NEE.**

To adapt the page to a new site:

1. Edit `D.VARS` in `core.js` (variables, depths, units).
2. Edit `deriveSim` (which model columns feed each variable).
3. Edit the file mapping in `build.js` (which observation columns feed each variable).
4. Change the page text in `template.html`.

## 9. Checks that were done

- Statistics were computed independently in Python (`check.py`) and in the R take-home script (`elora_compare.R`), and both matched the page.
- Before each publish: no console errors, and no horizontal scroll at 400 px width, in both light and dark mode.
