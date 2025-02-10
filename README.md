---
bibliography: references.bib
---

## Invasive alien plant data for ecosystem assessment

**National Biodiversity Assessment - South Africa**

*South African National Biodiversity Institute (SANBI)*

February 2025

#### Summary

Workflows for including invasive alien plant distribution and abundance data into ecosystem assessments for terrestrial ecosystems in South Africa. Part of the National Biodiversity Assessment 2025 led by the South African National Biodiversity Institute (SANBI). Each dataset on invasive alien density and distribution is crosstabulated (using the R terra package) with national land cover and national vegetation map data, resulting in tabular outputs that include the severity and extent of IAP per vegetation type. These outputs are then used in the Red List of Ecosystems assessment workflow in order to assess Criterion D (biotic disruption - functional decline) and to provide evidence of "ongoing decline" require in Criterion B assessments (limited distribution with ongoing decline).

Data included:

a)  National Invasive Alien Plant survey data (Kotze 2023) supplied by DFFE (Andrew Wannenburg)

b)  Western Cape Invasive Alien Tree survey (Rebelo 2024)

------------------------------------------------------------------------

### Workflow for using NIAPS 2023 data in RLE assessments for Criterion D3

[Workflow for NIAPS](Invasives_niaps.qmd)

This workflow uses new data on invasive alien plant species distribution and abundance to assess the severity and extent of functional decline of terrestrial ecosystems in South Africa (to support application of Criterion D of the Red List of Ecosystems v1.1).

The National Invasive Alien Plant Survey (NIAPS) run by the Department of Forestry, Fisheries and the Environment and Stellenbosch University (led by Dr Ian Kotze and Andrew Wanneburg), resulted in a series of raster datasets on distribution and abundance of selected taxa.

*This script must be run before the Western Cape analysis can be run as it supplies a table of naitonal remnant size.*

**Data sources & import:**

1.  South African National Land Cover data set for 2022 (prepared by the National Department of Forestry, Fisheries and the Environment) was modified by SANBI as described in Land Cover Change workflows. The data were reclassified in ARCGIS PRO into seven classes: 1 = Natural; 2 = Secondary Natural, 3 = Artificial water bodies, 4 = Built up, 5 = Croplands, 6 = Mines, 7 = Plantation (SANBI pers com).

2.  National Vegetation Map 2024 version 012025 vector data (ESRI file geodatabase), curated by SANBI was imported and then converted to a raster, snapped to the extent of the land cover.

3.  National Invasive Alien Plant Survey (NIAPS) (Kotze 2023) estimated the extent of the most-widespread & abundant, terrestrial invasive alien plant taxa (approx. 32 taxa) in South Africa. Data were downloaded from an ARCPRO package available [here](https://dffeportal.environment.gov.za/portal/home/item.html?id=17de13c509ef4d3caf279d84e77312c3). Each raster has pixel values (0-100) that are percentage of area invaded divided by condensed area invaded for 32 Invasive plant taxa organised into 13 rasters. Values of 100 represent 100% invasion (effectively 100% canopy cover of the specific invasive species)(see [@marais2004] for an explanation of the concept of "condensed area"). These rasters were stacked and the maximum value for each pixel was extracted. The raster was then projected to match extent, resolution and origin of the land cover data.

**Spatial Analysis**

The three rasters were cross tabulated (crosstab) in R terra and then converted to a [table](outputs/niaps_lc_veg_tb.csv) . This table was then summarised to produce the per vegetation type metrics of severity and extent of biotic distruption (by IAP) that are required by the RLE Criterion D assessments [(summary of NIAPS per vegetation type)](outputs/data_for_rle_niaps.csv)

### Workflow for using Cape Floristic Region IAT survey in RLE assessments for Criterion D3

[Workflow for CFR IAT](Invasives_rebelo_wc.qmd)

This workflow uses new data on invasive alien plant species distribution and abundance to assess the severity and extent of functional decline of terrestrial ecosystems in the Cape Floristic Region of South Africa (to support application of Criterion D of the Red List of Ecosystems v1.1). Rebelo et al. 2024 provided SANBI with a raster data set of invaded areas in the Cape Floristic Region (including a certainty band) [@rebelo].

*Note the niaps script must be run prior to this script as it supplies a table of national remnants sizes*.

**Data sources & import:**

1.  South African National Land Cover data set for 2022 (prepared by the National Department of Forestry, Fisheries and the Environment) was modified by SANBI as described in Land Cover Change workflows. The data were reclassified in ARCGIS PRO into seven classes: 1 = Natural; 2 = Secondary Natural, 3 = Artificial water bodies, 4 = Built up, 5 = Croplands, 6 = Mines, 7 = Plantation (SANBI pers com).

2.  National Vegetation Map 2024 version 012025 vector data (ESRI file geodatabase), curated by SANBI [@Dayaram2019] was imported and then converted to a raster, snapped to the extent of the land cover.

3.  Map of key woody invasive alien tree taxa within the Cape Floristic Region. This classification was generated using Sentinel-2 satellite imagery at a 10 m resolution using a Random Forest machine learning classifier. It follows a pure pixel approach - the majority of any pixel must have IAT canopy cover to be detected. The data is unprocessed and contain certainty information in Band 2 (values of 0-100) that incorporates fire scars and expert information.

**Spatial Analysis**

The land cover and vegetation were resmapled to match the extent, origin and resolution of the IAT data., and then all the rasters were cross tabulated (crosstab) in R terra and then converted to a [table](output/inv_wc_lc_veg_tb.csv). This table was then summarised to produce the per vegetation type metrics of severity and extent of biotic distruption (by IAT) that are required by the RLE Criterion D assessments ([summary of CFR IAT cover per vegetation type](outputs/data_for_rle_rebelo_invwc.csv)).
