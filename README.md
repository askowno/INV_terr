## Invasive alien plant data for terrestrial ecosystem assessment

**National Biodiversity Assessment - South Africa**

*South African National Biodiversity Institute (SANBI)*

February 2025

#### Summary

Workflows for including invasive alien plant distribution and abundance data into ecosystem assessments for terrestrial ecosystems in South Africa. Part of the National Biodiversity Assessment 2025. Data sets on invasive alien density and distribution were cross-tabulated (using the R terra package) with national land cover and the national vegetation map data, resulting in tabular outputs that include the severity and extent of IAP per vegetation type. These outputs are then used in the Red List of Ecosystems assessment workflow in order to assess Criterion D3 (biotic disruption - functional decline) and to provide evidence of "ongoing decline" require in Criterion B assessments (limited distribution with ongoing decline).

Data included:

a)  National Invasive Alien Plant survey data ([Kotze et al., 2025](https://doi.org/10.1007/s10530-025-03558-9)) supplied by DFFE (Andrew Wannenburg)

b)  Western Cape Invasive Alien Tree survey ([Rebelo et al. 2024](https://doi.org/10.25413/SUN.27377211))

c)  MAPWAPS invasive alien plant surveys for four catchments: Mzimvubu ([Skosana et al., 2024](https://doi.org/10.25413/SUN.25050401)), Tugela ([Cogill et al., 2024](https://doi.org/10.25413/SUN.25066151)) , Sabie-Crocodile ([Skosana et al., 2024](https://doi.org/10.25413/SUN.25050368)) and Luvuvhu ([Cogill et al., 2024](https://doi.org/10.25413/SUN.25050314)).

``` mermaid
flowchart LR; 
A[Land cover change data ARCGIS] --> B[INV_terr/Invasives_niaps.qmd] --> C(INV_terr/outputs/data_for_rle_niaps.csv) --> D[RLE D1 & D3 results]; 
E[National Invasive Alien Plant Survey] --> B; 
F[Vegetation map ARCGIS] --> B; 
G[CFR Invasive Alien Tree Survey] --> H[INV_terr/Invasives_rebelo_wc.qmd] --> I(INV_terr/outputs/data_for_rle_rebelo_invwc.csv) --> D; 
A --> H; 
F --> H;
A --> J;
F --> J;
K[MAPWAPS IAP Survey] --> J[INV_terr/Invasives_mapwap.qmd] --> L(Inv_terr/outputs/data_for_rle_mapwaps.csv) --> D;
```

------------------------------------------------------------------------

### 1. Workflow for using NIAPS 2023 data in RLE assessments for Criterion D3

[Workflow for NIAPS (Invasives_niaps.qmd)](Invasives_niaps.qmd)

This workflow uses new data on invasive alien plant species distribution and abundance to assess the severity and extent of functional decline of terrestrial ecosystems in South Africa (to support application of Criterion D of the Red List of Ecosystems v1.1).

The National Invasive Alien Plant Survey (NIAPS) run by the Department of Forestry, Fisheries and the Environment and Stellenbosch University (led by Dr Johann Kotze and Andrew Wannenburgh), resulted in a series of raster data sets on distribution and abundance of selected taxa.

**Data sources & import:**

1.  South African National Land Cover data set for 2022 (prepared by the National Department of Forestry, Fisheries and the Environment) was modified by SANBI as described in Land Cover Change workflows. The data were reclassified in ARCGIS PRO into seven classes: 1 = Natural; 2 = Secondary Natural, 3 = Artificial water bodies, 4 = Built up, 5 = Croplands, 6 = Mines, 7 = Plantation (SANBI pers com).

2.  National Vegetation Map 2024 version 012025 vector data (ESRI file geodatabase), curated by SANBI was imported and then converted to a raster, snapped to the extent of the land cover.

3.  National Invasive Alien Plant Survey (NIAPS) ([Kotze et al., 2025](https://doi.org/10.1007/s10530-025-03558-9)) estimated the extent of the most-widespread & abundant, terrestrial invasive alien plant taxa (approx. 32 taxa) in South Africa. Data were downloaded from an ARCPRO package available [here](https://dffeportal.environment.gov.za/portal/home/item.html?id=17de13c509ef4d3caf279d84e77312c3). Each raster has pixel values (0-100) that are percentage of area invaded divided by condensed area invaded for 32 Invasive plant taxa organised into 13 rasters. Values of 100 represent 100% invasion (effectively 100% canopy cover of the specific invasive species)(see [Marais et al., 2004](https://journals.co.za/doi/abs/10.10520/EJC96205) for an explanation of the concept of "condensed area"). These rasters were stacked and the maximum value for each pixel was extracted. The raster was then projected to match extent, resolution and origin of the land cover data.

**Spatial Analysis**

The three rasters were cross tabulated (crosstab) in R terra and then converted to a [table](outputs/niaps_lc_veg_tb.csv) . This table was then summarised to produce the per vegetation type metrics of severity and extent of biotic disruption (by IAP) that are required by the RLE Criterion D assessments [(summary of NIAPS per vegetation type)](outputs/data_for_rle_niaps.csv)

### 2. Workflow for using Cape Floristic Region IAT survey in RLE assessments for Criterion D3

[Workflow for CFR IAT (Invasives_rebelo_wc.qmd)](Invasives_rebelo_wc.qmd)

This workflow uses new data on invasive alien tree species distribution and abundance to assess the severity and extent of functional decline of terrestrial ecosystems in the Cape Floristic Region of South Africa (to support application of Criterion D of the Red List of Ecosystems v1.1). Alanna Rebelo (Agricultural Research Council and Stellenbosch University) provided SANBI with a raster data set of invaded areas in the Cape Floristic Region (including a certainty band).

**Data sources & import:**

1.  South African National Land Cover data set for 2022 (prepared by the National Department of Forestry, Fisheries and the Environment) was modified by SANBI as described in Land Cover Change workflows. The data were reclassified in ARCGIS PRO into seven classes: 1 = Natural; 2 = Secondary Natural, 3 = Artificial water bodies, 4 = Built up, 5 = Croplands, 6 = Mines, 7 = Plantation (SANBI pers com).

2.  National Vegetation Map 2024 version 012025 vector data (ESRI file geodatabase), curated by SANBI (Dayaram et al., 2019) was imported and then converted to a raster, snapped to the extent of the land cover.

3.  Map of key woody invasive alien tree taxa within the Cape Floristic Region ([Rebelo et al. 2024](https://doi.org/10.25413/SUN.27377211)). This classification was generated using Sentinel-2 satellite imagery at a 10 m resolution using a Random Forest machine learning classifier. It follows a pure pixel approach - the majority of any pixel must have IAT canopy cover to be detected. The data is unprocessed and contain certainty information in Band 2 (values of 0-100) that incorporates fire scars and expert information.

**Spatial Analysis**

The land cover and vegetation were resmapled to match the extent, origin and resolution of the IAT data., and then all the rasters were cross tabulated (crosstab) in R terra and then converted to a [table](output/inv_wc_lc_veg_tb.csv). This table was then summarised to produce the per vegetation type metrics of severity and extent of biotic distruption (by IAT) that are required by the RLE Criterion D assessments ([summary of CFR IAT cover per vegetation type](outputs/data_for_rle_rebelo_invwc.csv)).

### 3. Workflow for using MAPWAPS survey data in RLE assessments for Criterion D1

[Workflow for MAPWAPS (Invasives_mapwap.qmd)](Invasives_mapwap.qmd)

This workflow uses new data on invasive alien plant species distribution and abundance to assess the severity and extent of functional decline of terrestrial ecosystems in four catchments areas in north eastern South Africa (to support application of Criterion D of the Red List of Ecosystems v1.1). Cogill et al. & Skosana et al. provided SANBI with a vector data set of invaded areas in the Sabie-Crocodile, Luvuvhu, uMzimvubu and Tugela catchment areas.

**Data sources & import:**

1.  South African National Land Cover data set for 2022 (prepared by the National Department of Forestry, Fisheries and the Environment) was modified by SANBI as described in Land Cover Change workflows. The data were reclassified in ARCGIS PRO into seven classes: 1 = Natural; 2 = Secondary Natural, 3 = Artificial water bodies, 4 = Built up, 5 = Croplands, 6 = Mines, 7 = Plantation (SANBI pers com).

2.  National Vegetation Map 2024 version 012025 vector data (ESRI file geodatabase), curated by SANBI (Dayaram et al., 2019) was imported and then converted to a raster, snapped to the extent of the land cover.

3.  Map of key invasive alien plant taxa within Mzimvubu ([Skosana et al., 2024](https://doi.org/10.25413/SUN.25050401)), Tugela ([Cogill et al., 2024](https://doi.org/10.25413/SUN.25066151)) , Sabie-Crocodile ([Skosana et al., 2024](https://doi.org/10.25413/SUN.25050368)) and Luvuvhu ([Cogill et al., 2024](https://doi.org/10.25413/SUN.25050314)) catchment areas collected as part of the MAPWAPS program. This classification was generated using Sentinel-2 satellite imagery at a 10 m resolution using a Gradient Tree Boost (GTB) classification. The vector data were pre processed in ARCGIS PRO, where an attribute [mapwaps] was added to each data set. Where LUCL = Alien\* the value 8 was assigned, otherwise the value 0. This allows for determination of invaded areas and other areas within the area of interest. The shapefiles for Luvuhvu and and Sabie-Crocodile were projected from UTM36S to UTM35S, and then all four were merged into a single shapefile mapwaps.shp. this was then rasterized (using mapwaps attribute) and snapped to the national land cover grid 2022 (including projection from UTM to Albers Equal Area to match national land cover and vegetation data).

**Spatial Analysis**

The land cover and vegetation were resampled to match the extent, origin and resolution of the IAP data., and then all the rasters were cross tabulated (crosstab) in R terra and then converted to a [table](output/inv_wc_lc_veg_tb.csv). This table was then summarised to produce the per vegetation type metrics of severity and extent of biotic disruption (by IAT) that are required by the RLE Criterion D assessments ([summary of MAPWAPS invasive cover per vegetation type](outputs/data_for_rle_mapwaps.csv)).

### 4. Workflow for combining all invasive alien plant data for use in Ecosystem Protection Level and Red List of Threatened SPecies assessments. 

[Workflow for combining all IAP data (Invasives_combined.qmd)](Invasives_combined.qmd)

This workflow combines invasive alien plant data from the data sources described in section 1-3 into single raster coverage. This will be used as an additional input layer in updated Red List of Threatened Plant and Amphibian assessments, and terrestrial Ecosystem Protection Level Assessments for the National Biodiversity Assessment 2025. The data were prepossessed in ARCGIS PRO and recoded such that all pixels for which invasives were detected were assigned value of 8, all other pixels were assigned value of 1.

#### Analysis and outputs: 

The three invasives rasters were combined such that any pixel of value = 8 (invaded) was retained and all other pixels were assigned value = 0 (not invaded) to produce a **inv_comb.tif** raster. This data does not take into account the existing land cover, and invasions within secondary natural areas and urban area are retained.

This combined invasion raster was combined with the 7 class national land cover such that if land cover was = 1 (natural) and the combined invasion layer = 8 (invaded) then the output pixel value was = 8. If not then the land cover value was assigned. This results in a 8 class national land cover : 1 = natural, 2 = secondary natural(old fields), 3 = artificial water bodies, 4 = buyilt up (infrastructure), 5 = croplands and orchards, 6 = mines and mine dumps, 7 = plantation forestry, 8 = invasive alien plant (high density). The output is a raster **lc2022_inv.tif**
