# GEE-Project: Sentinel-1/Sentinel-2 Land Cover Classification

Google Earth Engine scripts for supervised land cover classification using a fusion of Sentinel-2 optical and Sentinel-1 SAR data, with Random Forest classification across multiple time periods.

## Overview

Each script in this repository follows the same core pipeline:

1. **Sentinel-2 optical processing** — filters by date, region of interest (ROI), and cloud cover (`CLOUDY_PIXEL_OVER_LAND_PERCENTAGE`), then computes:
   - **NDVI** (Normalized Difference Vegetation Index) from bands B8/B4
   - **NDWI** (Normalized Difference Water Index) from bands B3/B8
   - 75th and 55th percentile composites to reduce cloud/noise artifacts

2. **Sentinel-1 SAR processing** — filters by date, ROI, VV/VH polarization, IW instrument mode, and descending orbit pass, then takes percentile composites of VV/VH backscatter.

3. **Data fusion** — combines the optical (NDVI/NDWI + bands) and SAR (VV/VH) percentile composites into a single multi-band dataset.

4. **Supervised classification** — trains an `ee.Classifier.smileRandomForest` (80 trees) on manually labeled training samples, then classifies the full dataset.

5. **Export** — classified land cover maps, NDVI, and NDWI layers exported to Google Drive as GeoTIFFs (EPSG:4326, 30m scale).

## Scripts

| Script | Date Range | Classes | Notes |
|---|---|---|---|
| `NDVi_Change.js` | 2020-03 to 2020-04 | 4 classes (Waterbody, Agricultural Land, Vegetation, Fallow Land) | Baseline classification and NDVI/NDWI computation |
| `Supervised_1111.js` | 2024-09 to 2024-12 | 6 classes (+ Settlements, Barren Land) | Full pipeline with area calculation (sq. km per class), bar chart, and CSV export |
| `Supervised_CNF_0301.js` | 2024-03 to 2024-04 | 6 classes | Streamlined export-only version |

## Land Cover Classes

- Waterbody
- Vegetation
- Agricultural Land
- Fallow Land
- Settlements *(6-class scripts only)*
- Barren Land *(6-class scripts only)*

## Data Sources

- **Sentinel-2 SR/TOA** (`imageCollection`) — optical bands B2–B8
- **Sentinel-1 GRD** (`imageCollection2`) — VV/VH polarization, IW mode, descending orbit
- Training samples digitized as `FeatureCollection`s per class (e.g. `waterbody`, `Vegetation`, `Agricultural_Land`, etc.) within Earth Engine

## How to Run

1. Open the script in the [GEE Code Editor](https://code.earthengine.google.com)
2. Define or import: `roi` (region of interest), training sample `FeatureCollection`s per class, and the Sentinel-1/Sentinel-2 `ImageCollection` imports
3. Run the script — classified map, NDVI, and NDWI layers will appear on the map
4. Uncomment the `Export.image.toDrive()` / `Export.table.toDrive()` blocks and run them from the **Tasks** tab to export results

## Background

These scripts were developed as part of inundation and land cover change analysis using satellite remote sensing, applying SAR-optical fusion techniques to improve classification accuracy in regions affected by seasonal flooding and land use change in Bangladesh.

## Author

Sourav Mollick
