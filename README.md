# CCAR Tree & Stump Spatial Analysis

**Stone Valley Forest | Penn State University**

## Project Overview

This project visualizes and analyzes GPS-collected tree and stump locations in **Stone Valley Forest** using Python-based geospatial tools. The workflow converts raw field GPS data into an interactive, layered map that supports forest inventory analysis, spatial uncertainty, and shortest-path navigation between tagged features.

The repository is designed to be **fully reproducible**, cloud-independent, and compatible with **VS Code, Jupyter, and GitHub**.

---

## Data Acquisition

Data were collected through a **lab internship** and consist of:

* ~30 raw **`.gpx` files** containing GPS coordinates
* One **Excel spreadsheet** containing attribute data such as:

  * Species
  * DBH
  * Block designation (Harvest vs Control)
  * Tag identifiers

Each GPX file corresponds to a unique tag or set of related coordinates recorded in the field using consumer-grade GPS units.

---

## Data Analysis

1. All files ending in `.gpx` within the `Coordinates/` folder are automatically discovered and imported into the notebook.
2. Using `gpxpy`, each file is parsed to extract:

   * Latitude
   * Longitude
   * Tag#
3. These values are assembled into a master dataframe containing **2,510 GPS points**.
4. Tags containing **`S` or `SNT`** are classified as **stumps**.
5. Two primary datasets are created:

   * `no_stumps` (trees)
   * `stumps`
6. Additional subsets are generated based on:

   * Block type (Harvest vs Control)
   * Species group (Oaks vs Other Species)

This structure allows efficient filtering and layered visualization.

---

## Visualization

Using **GeoPandas** and **Folium**, GPS points are projected and visualized in their real-world locations.

### Symbology

* **Blue points** → Trees
* **Red points** → Stumps
* **Gray buffers (9 ft radius)** → GPS uncertainty zone
* **Orange points** → Harvest block
* **Green points** → Control block
* **Yellow points** → Oak species
* **Purple points** → Other species

Each layer can be toggled on/off, allowing focused spatial analysis.

---

## Coordinate Reference Systems (CRS)

* **EPSG:4326 (WGS84)**

  * Used for plotting latitude and longitude on web maps
* **EPSG:2272 (PA South StatePlane, feet)**

  * Used for buffer creation and distance calculations

This CRS switching ensures both **accurate visualization** and **real-world distance integrity**.

---

## GPS Buffering

Each point is surrounded by a **9-foot buffer** to account for spatial drift common in consumer-grade GPS units.

This buffer represents the realistic search area for locating a physical tree or stump tag in the field. Users navigating to a coordinate should search within this radius and verify the physical tag against recorded attributes.

---

## Shortest Path Analysis

A shortest-path function was implemented to determine the optimal travel route between two tagged locations.

This analysis:

* Uses **trees only** (no stumps) for contextual awareness
* Plots an **orange path** representing the optimal route
* Uses:

  * `pandas`
  * `geopandas`
  * `shapely`
  * `folium`
  * `LocateControl`
  * `math`

### Functionality

* Calculates straight-line distance between points
* Determines cardinal travel direction
* Displays the route alongside surrounding tree data for interpretation

Both **EPSG:4326** and **EPSG:2272** are used to balance geographic plotting and physical distance accuracy.

---

## Documentation & Notebook Structure

### Section 1 — Environment Setup

* Installation of required packages:

  * `geopandas`
  * `folium`
  * `gpxpy`
* Notebook configured to run locally (no Google Drive dependency)

### Section 2 — GPX Parsing

* Automatic discovery of `.gpx` files
* Parsing of latitude, longitude, and tag data
* Assembly into a master dataset

### Section 3 — Stump Classification

* Tags containing `S` or `SNT` classified as stumps
* Separation into stump and non-stump dataframes

### Section 4 — Attribute Merging

* Coordinate data merged with Excel attribute data
* Join performed on `Tag#`
* Enables species and block-based filtering

### Section 5 — Layered Mapping

* Interactive Folium map
* Toggleable layers for different data subsets
* CRS explicitly defined for transparency

### Section 6 — Spatial Buffering

* 9-foot buffer applied to all points
* Accounts for GPS drift
* Implemented in a feet-based CRS

### Section 7 — Shortest Path Analysis

* Calculates optimal routes between selected tags
* Visualizes distance and direction
* Useful for navigation and field planning

---

## Repository Structure

```
CCAR-Map/
├── CCARMAPGH_localpaths.ipynb
├── Coordinates/
│   ├── *.gpx
├── requirements.txt
└── README.md
```

---

## Notes on Notebook Trust

Interactive maps require the notebook to be **trusted** in Jupyter or VS Code.

If maps do not render:

* Click **“Trust Notebook”** when prompted
* Or run:

```bash
jupyter trust CCARMAPGH_localpaths.ipynb
```

---

## Author

**Colin J. Fetterman**
Forestry Major — Penn State University
