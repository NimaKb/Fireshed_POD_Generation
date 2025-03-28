# Scripts for Fireshed Scoring Calibration and POD HTML Map Generation

## Overview
This folder contains scripts used for fireshed scoring calibration and POD (Potential Operational Delineation) HTML map generation. These scripts facilitate the processing, analysis, and visualization of fire risk data for the Cariboo Region. The outputs include calibrated fire risk scores and interactive HTML maps for operational planning.

### Structure
```
scripts/
├── calibration/               # Fireshed scoring calibration scripts
│   └── fireshed_calibration.R  # R script for fireshed cumulative score calibration
├── html_generation/           # POD HTML map generation scripts
│   └── pods_html_map.R         # R script to create interactive HTML maps
```

---

## Calibration Script: fireshed_calibration.R
This script performs cumulative score calibration for firesheds using wildfire data. It generates density plots based on the burn count for three predefined areas:
- **Williams Lake**
- **Quesnel**
- **Yunesit’in**

### How to Run
Execute the script from the command line or in RStudio:
```bash
Rscript scripts/calibration/fireshed_calibration.R
```

### User Input
The script will prompt the user to select the area and the path to the DBF file:
```
Select the area for calibration (Williams Lake, Quesnel, Yunesit'in):
Williams Lake
Enter the path to Williams Lake DBF file:
data/fireshed_data/Calibration_Williams.dbf
```

### Calibration Logic
1. **Area Selection:** The script prompts the user to choose one of the three areas: Williams Lake, Quesnel, or Yunesit'in.
2. **File Path Input:** The user provides the file path to the DBF file.
3. **Burn Count Grouping:** The script dynamically sets the burn count grouping based on the selected area:
   - Williams Lake: `0-60`, `60-120`, `120-180`
   - Quesnel: `0-27`, `27-54`, `54-81`, `81-108`
   - Yunesit'in: `0-100`, `100-200`, `200-300`, `300-400`
4. **Density Plot Generation:** For each burn count group, the script generates density plots with the peak density marked using a dashed line.
5. **Plot Combination:** The final plot is created by combining density plots for all burn count groups.

### Outputs
- A multi-panel density plot illustrating the relationship between burn counts and cumulative scores.
- Separate plot files for each area, saved in the output directory.

### Example Output
```
Burn Count: 0-60
Burn Count: 60-120
Burn Count: 120-180
```

---

## POD HTML Map Generation Script: pods_html_map.R
This script generates interactive HTML maps to visualize fire risk and potential operational delineations (PODs). It uses GeoJSON data to create map layers and visualize fire threats, fuel distribution, terrain, and climate anthropogenic influences.

### How to Run
Execute the script from the command line or in RStudio:
```bash
Rscript scripts/html_generation/pods_html_map.R
```

### User Input
The script will prompt the user for:
1. The directory containing the GeoJSON file.
2. The file name of the GeoJSON file.
3. The output HTML file name.

### Outputs
- An interactive HTML map that visualizes fire risk and POD boundaries.
- The map includes multiple layers with toggle options for base maps and overlays.

---

## Best Practices
Ensure all necessary packages are installed using:
   ```r
   if (!requireNamespace("pacman", quietly = TRUE)) install.packages("pacman")
   pacman::p_load(foreign, dplyr, tibble, ggplot2, ggthemes, scales, patchwork, leaflet, geojsonio, htmlwidgets)
   ```
## Troubleshooting
- Check file paths and ensure that the necessary files are present in the expected directories.
- Verify that the required R packages are installed and loaded.

## Contact
For any issues or questions, please contact **Nima Karimi** at nima.karimi@forsite.ca.
