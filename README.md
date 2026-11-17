# DNDC-Training
Training Repository containing the Model executable, example site files and training modules

**DNDC_HPC** (High Performance Cluster Version of DNDCv.CAN for WINDOWS and LINUX versions)

This is the High Performance Cluster Version of CANADA DNDC (DNDCv.CAN). It can be run in LINUX with DNDCgo.exe or Windows depending on the Cmake compile. There is no GUI available with the HPC version of the model and it requires the user to understand the intended structure of the DNDC model input file (.dnd). If you need to make a new .dnd file and are unsure how to edit it then use the windows GUI version of the model (see below) to create and save a dnd file and then run with the HPC version.

**DNDC_GUI** (This is classic DNDCv.CAN with a graphic user interface)

This has a GUI version built off the legacy version of DNDC95 and has been updated to support DNDC.vCAN. The model can be run using windows for a single site and for multiple sites at once using either site run or batch run commands. There is also a command line version of this windows version that can be run but it has been made obsolete by the DNDC_HPC command line version of the model. The DNDCv.CAN (DNDC_Windows) will be slowly shelved in developement and only the DNDC_HPC version will be supported for new development additions.

## Model files for you to download and extract to your computer

Please download and unzip **DNDC_HPC_NOV2026.zip** to your root C:\  
It should extract to C:\DNDC_WIN
the executable for windows will be named C:\DNDC\DNDCgo.exe
This will be a commmand prompt version of the model.

Please unzip **DNDC_GUI_NOV2026.zip** to C:\
It should extract to C:\DNDC
The excutable for windows will be name C:\DNDC\DNDC95.exe
This is the classic Graphic User Interface version of DNDC. It has menus that you need to navigate to open *.dnd files

# DNDC-Training - Examples

Both the DNDC_HPC and the DNDC_GUI versions have pre-constructed dnd files we will can open and manipulate and run for this workshop.
DNDC_HPC will need to be run through the commmand prompt. The step1-step5 bat files will auto populate the command prompt to run the model for the example. 
DNDC_GUI will need to open up the .dnd file by going through the site input menu and loading a .dnd file.

DNDC_HPC model results will be analyzed using a AI tool to demonstrate how one can investigate different outcomes from the model using measured data. We will be using the included DNDC-Training Site 1 that is located in southern Ontario. This is for demonstration purposes only and the observed data is not provided for analysis. The AI tool can be opened using the following link.

**LINK TO TOOL FOR MODEL RESULT ANALYSIS**
https://claude.ai/artifact/8gM5h4V33ax2abM6gpR4Jz


The example data we will be using is strictly for demonstration purposes and is not meant to be used in publication


The site is in Ontario Canada and is managed by Claudia Wagner-Riddle cwagnerr@uoguelph.ca
# DNDC-Training Site 1

Southern Ontario Research Station, current plot 1 (`Elora.dnd`, site name `Elora_P1_history`), 2000-2023.

## Source
- Citation: management compiled from Wagner-Riddle et al. (2007), Jayasundara et al. (2006, 2007),
  Congreves et al. (2016), Sulaiman et al. (2017), Machado et al. (2020a), Tariq et al. (2024), station
  logbooks and research station records (references per year in the workbook below).
- Data owner / contact: to be confirmed
- Management records: `Elora_Management_Dec2024.xlsx` (`C:\DNDC\sites\Elora20y`): sheets
  `Summary of Agronomy New` (all plots, 2000-2024) and `Plot1_20yr_longterm` / `Plot3_20yr_longterm`.
- Data rights: Shared with permission of site owner. Do not publish without obtaining authorization first.

## Location
- Latitude, longitude: 45.45 N as entered in `Elora.dnd` (longitude not recorded here)
- Soil series / texture: loam in the .dnd (texture 4, clay 0.19, bulk density 1.26, pH 7.0, top-layer
  SOC 0.0257). `10cm-soil.spf` has 8 layers to 2 m: bulk density 1.25-1.40, SOC 2.6% in the top 15 cm
  falling to 0.1% at depth.

## Plots
`Elora.dnd` and the observations cover plots 1 and 3. The BMP plots (2 and 4) are not set up yet.

The plots were renumbered over the years. The workbook's long-term sheets trace current plot 1 back
through old plot 2* (2000-2011) and old plot 3* (2012-2014); current plot 3 goes back through old
plot 3*. Plots 1 and 3 had the same management until 2014 and differ from 2015:

| years | current plot 1 (what `Elora.dnd` models) | current plot 3 |
|-------|-------------------------------------------|----------------|
| 2000-2011 | BMP: no-till, corn-soybean-winter wheat | same |
| 2012-2014 | manure application-time experiment: fall manure, corn | same (urea + fall manure per station records) |
| 2015-2017 | corn, broadcast urea + NUI | corn, UAN + NUI injected at side-dress |
| 2018-2023 | conventional rotation: corn-soy-soy-corn-soy-soy | diverse rotation: winter wheat and cover crops |

## Period and management
- Years observed: 2000-2023; 2015-2023 is the high-confidence period.
- Plot 1 management (from `Plot1_20yr_longterm`):

| year | crop | N (kg N/ha) | tillage |
|------|------|-------------|---------|
| 2000 | corn | UAN 50 injected Jul 4 + MAP starter 7.4 | no-till |
| 2001 | soybean; winter wheat seeded Oct 2 | none | no-till |
| 2002 | winter wheat (harvest Aug 1); red clover under-seeded Apr 23 | urea 60 (30 credited from soybean) | no-till; straw removed |
| 2003 | corn (red clover killed Apr 30) | UAN 60 side-dress Jul 2 + MAP starter 6.2 | no-till |
| 2004 | soybean | none | no-till |
| 2005 | corn | urea 98 + MAP starter 6.2 | no-till |
| 2006 | soybean | none | no-till |
| 2007 | corn | urea 98 + MAP starter 6.2 | no-till |
| 2008 | corn | urea 120 + MAP starter 6.2 | not recorded; residue added |
| 2009 | corn | UAN 118 side-dress Jun 23 + MAP starter 6.2 | no-till |
| 2010 | soybean; winter barley seeded Oct 1 | none | no-till |
| 2011 | winter barley (harvest Jul 14) | urea 98 | no planting |
| 2012 | corn | fall manure 81 (Oct 19) | plough Apr 16, disc Apr 18 and with manure |
| 2013 | corn | fall manure 106 (Nov 13) | disc |
| 2014 | corn | urea 60 (Jun 27) + fall manure 107 (Nov 28) | disc |
| 2015-2017 | corn | urea + NUI 120 broadcast, incorporated at planting + starter 30 | plough and disc |
| 2018 | corn | urea 154 + MAP starter 6.2 | disc |
| 2019 | soybean | MAP/MOP blend 9.4 | plough, disc |
| 2020 | soybean | none | no-till |
| 2021 | corn | UAN 162 injected Jun 17 + starter 5 | disc |
| 2022 | soybean | none | no-till |
| 2023 | soybean | none | no-till |



