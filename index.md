# CAMELS-CH reproducibility guide

The purpose of this guide is to describe the creation of the CAMELS-CH dataset for repoducibility and to faciliate future updating and customized application. Corresponding codes are generally available at https://github.com/camels-ch/camels and specified in each attribute category below.

## Table of Contents

<!-- TOC start -->
- [Conventions on file content](#conventions-on-file-content)
- [Location and topography](#location-and-topography)
- [Climate](#climate)
- [Hydrology](#hydrology)
- [Soil](#soil)
- [Hydrogeology](#hydrogeology)
- [Geology](#geology)
- [Glacier](#glacier)
- [Land cover](#land-cover)
- [Human impact](#human-impact)
<!-- TOC end -->

## Conventions on file content

The following conventions have been defined for the resulting files:

* The first column contains the catchment id
* Missing data are tagged as: NA
* Precision for real data: by default two decimal places. Higher precision can be used when needed.
* Separators to use are semicolon (;)
* Avoid using any coma in the text (use hyphen if needed)
* No quote (") even for the text
* Timestamp: YYYY-MM-DD
* Encoding: UTF-8
* End of line: Unix (LF)

## Location and topography

**Source data**:
* Main data source: Gauge properties and catchement shapefiles provided by BAFU
* Complemented with the EU-Hydro river network database (https://land.copernicus.eu/imagery-in-situ/eu-hydro)
* Elevation and slope: European Digital Elevation Model (EU-DEM), version 1.1: https://land.copernicus.eu/imagery-in-situ/eu-dem/eu-dem-v1.1?tab=download&selected:list=dem-v1-1-e40n20

**Instructions**:
* Computation of the mean and percentiles of the elevation: In ArcGIS Pro Spatial Analyst Tools: ZonalStatistics as Table (mean; percentile_values=[10,25,50,75,90])

**Contributors**: Rosi Siber

## Climate

**Versions**: Climatic indices are calculated based on i) observed data, and ii) simulated data.

**Source data**:
* Observed data: Temperature and precipitation time series are from MeteoSwiss, and potential evapotranspiration time series are from Prevah (and thus simulated). The observation-based climatic indices were calculated for the same time period as the observation-based hydrological signatures to ease comparison.
* Simulated data: Streamflow time series are from Prevah simulations. The simulation-based climatic indices were calculated for the same time period as the simulation-based hydrological signatures to ease comparison.

**Code used**: 
* hydro_climate_attributes/climate_indices.R

**Instructions**:
* Note: Only years with complete hydrological year are used (5% missing values are tolerated per hydrological year).
* Note: The variables "high_prec_timing" and "low_prec_timing" can have NA values if the number of high precipitation days or the number of low precipitation days is identical in multiple seasons.

**Contributors**: Sandra Pool, Marvin Höge

## Hydrology

**Versions**: Hydrological signatures are calculated based on i) observed data, and ii) simulated data.

**Source data**:
* Observed data: Streamflow time series provided by BAFU. The length of the observed time series varies among catchments.
* Simulated data: Streamflow time series are from Prevah simulations. The simulated time series is identical for all catchments and lasts from 1981-10-01 to 2020-09-30.

**Code used**: 
* hydro_climate_attributes/hydro_signatures.R

**Instructions**:
* Note: Only years with complete hydrological year are used (5% missing values are tolerated per hydrological year).
* Note: The variable "stream_elas" can have a value of NA if only one complete hydrological year is available.

**Contributors**: Sandra Pool, Marvin Höge


## Soil

**Attributes description**:
From SoilGrid we get
-------------------------
for 7 layers resp. depths (0 cm,5 cm,15 cm,30 cm,60 cm,100 cm,200 cm)
* sand percentage[%]  <=> Weight percentage of the sand particles (0.05-2 mm) SNDPPT [%]
* silt percentage [%]  <=> Weight percentage of the silt particles (0.0002-0.05 mm) SLTPPT [%]
* clay percentage[%]  <=> Weight percentage of the clay particles (<0.0002 mm) CLYPPT [%]
* percentage organic content [%]  <=> 	Soil organic carbon content ORCDRC [permille]/10
* bulk density [g/cm3]  <=> bulk density BLDFIE [kg/m3]*1000
* total available water content [mm]  <=> AWCh1 [%]/100*1000
 from AWCh1 [%] Available soil water capacity (volumetric fraction) with FC = pF 2.0
(see also [googlegroup SoilGrid AWCh1](https://groups.google.com/g/global-soil-information/c/NNwCT3mqXRM/m/vP-qjJpdBQAJ))
Total available water (TAW) is defined as the difference between the water content at field capacity (FC) and at wilting point (WP)
(FC = AWCH1 + WWP; see also [googlegroup SoilGrid FC](https://groups.google.com/g/global-soil-information/c/SMJzAXBhS08/m/Lc15MjMyBQAJ))
for the whole soil profile (1 layer)
* soil depth [m] <=> absolute depth to bedrock BDTICM [cm]/10
* root depth [m] <=> depth to bedrock (R horizon) up to 200cm BDRICM [cm]/10

See also [FAQ SoilGrid](https://www.isric.org/explore/soilgrids/faq-soilgrids-2017) for more information (there are also more available data)

From EU-SoilHydroGrids we get
-------------------------------
For 7 layers resp. depths (0 cm,5 cm,15 cm,30 cm,60 cm,100 cm,200 cm)
* porosity [-] <=> saturated volumetric water content THS [cm3/cm3] *100 / 100 
* conductivity [cm/h] <=> saturated hydraulic conductivity KS [cm/day] *100 /(100*24)

From European Soil Database Derived data we get
-------------------------------------------------
For topsoil(T; first 30 cm) and subsoil(S; root depth-30cm)
* sand percentage[%]  <=> Sand content STU_EU_T&S_SAND [%]
* silt percentage [%]  <=> Sand content STU_EU_T&S_SILT [%]
* clay percentage[%]  <=> Sand content STU_EU_T&S_CLAY [%]
* percentage organic content [%]  <=> 	Organic carbon content STU_EU_T&S_OC [%]
* bulk density [g/cm3]  <=> bulk density STU_EU_T&S_BD [g/m3]
* total available water content [mm]  <=> Total available water content from PTF STU_EU_T&S_TAWC[mm]
* coarse fragments [%]  <=> Coarse fragments  STU_EU_T&S_GRAVEL [%]
For the whole soil profile (1 layer)
* root depth [m] <=> depth available to roots STU_EU_DEPTH_ROOTS [cm]/100

For Europe alternative data sources for some attributes can be found also here: [alternative EU data](https://esdac.jrc.ec.europa.eu/content/spade-14/)

**Source data**:
* [SoilGrid](https://files.isric.org/soilgrids/former/2017-03-10/data/), refer also to [SoilGrid github](https://github.com/ISRICWorldSoil/SoilGrids250m); [ESSD](https://esdac.jrc.ec.europa.eu/content/european-soil-database-derived-data)

**Code used**: 
* soil_attributes/extract_soildata_EU.R
* soil_attributes/produce_soildata_EU.R

**Instructions**:
1. Either download manually data from [SoilGrid](https://files.isric.org/soilgrids/former/2017-03-10/data/), or download data using WCS from [SoilGrids WCS](https://soilgrids.org/). See [Tutorial SoilGrids WCS](https://git.wur.nl/isric/soilgrids/soilgrids.notebooks/-/blob/master/markdown/wcs_from_R.md) for a tutorial. Save data in Sys.getenv('CAMELS_DIR_DATA')/Soil/SoilData/SoilGrid/data.
2. Submit a request and get data of the 3D soil hydraulic database of Europe under [EU-SoilHydroGrids](https://esdac.jrc.ec.europa.eu/content/3d-soil-hydraulic-database-europe-1-km-and-250-m-resolution). Save data in Sys.getenv('CAMELS_DIR_DATA')/SoilData/EU_SoilHydroGrids_250m and/or Sys.getenv('CAMELS_DIR_DATA')/SoilData/EU_SoilHydroGrids_1km.
3. Submit a request and get data of the European Soil Database Derived data under [ESSD](https://esdac.jrc.ec.europa.eu/content/european-soil-database-derived-data). Save data in Sys.getenv('CAMELS_DIR_DATA')/Soil/SoilData/STU_EU_Layers.
4. You are all done to run the code!

**Contributors**: Martina Kauzlaric


## Hydrogeology

**Attributes description**:
* hygeol_unconsol_coarse_perc: unconsolidated 1 (well-permeable gravel in valley bottoms)
* hygeol_unconsol_medium_perc: unconsolidated 2 (permeable gravel outside of valley bottoms, sandy gravel, medium- to coarse-grained gravel)
* hygeol_unconsol_fine_perc: unconsolidated 3 (loamy gravel, fine- to medium-grained debris, moraines)
* hygeol_unconsol_imperm_perc: impermeable, unconsolidated (clay, silt, fine sands, loamy moraines)
* hygeol_karst_perc: karstic rock (carbonate rock: limestone, dolomite, rauhwacke; sulphate-containing rock: gypsum, anhydrite)
* hygeol_hardrock_perc: hard rock (fissured and porous, non-karstic hard rock: conglomerates, sandstone, limestone with marl layers; crystalline rock: granite, granodiorites, tonalite)
* hygeol_hardrock_imperm_perc: impermeable hard rock (marl, shale, gneiss, cemented sandstone)
* hygeol_water_perc: water (glaciers, firn, surface waters)
* hygeol_external_perc: external (not defined, or outside of original map source)

**Source data**:
* [Hydrogeologische Karte der Schweiz: Grundwasservorkommen 1:500000](https://data.geo.admin.ch/ch.swisstopo.geologie-hydrogeologische_karte-grundwasservorkommen), reclassified according to [Viviroli (2007)](https://boris.unibe.ch/165989) and [Viviroli et al. (2009)](https://dx.doi.org/10.1016/j.jhydrol.2009.08.022)

**Code used**: 
* hydrogeologic_attributes/extract_hydrogeol_CH.R
* hydrogeologic_attributes/produce_hydrogeol_CH.R

**Instructions**:
1. Download manually either from [GeoMaps 500 Vector](https://www.swisstopo.admin.ch/en/geodata/geology/maps/gk500/vector.html) for V1_2 as vector data, or both vectorised and raster data v1_3 from [Hydrogeologische Karte der Schweiz: Grundwasservorkommen 1:500000](https://data.geo.admin.ch/ch.swisstopo.geologie-hydrogeologische_karte-grundwasservorkommen/)
2. Save data in Sys.getenv('CAMELS_DIR_DATA')/Hydrogeology/GK500_V1_3_DE. You find the data you need in the subdirectory LV95/Shapes_LV95 => shapefile PY_Basis_Flaechen.shp. In this shapefile the relevant field for defining hydrogeological properties is H2_ID it contains information on the groundwater resources: the aquifer, the hydrogeology and the productivity
3. We reclassify here this field as follows:
 H2_ID   ATTRIBUTE NAME                DESCRIPTION AND DETAILS
 0       hygeol_null_perc              not defined --> polygons without defined hydrogeology
 1,2     hygeol_unconsol_coarse_perc   unconsolidated coarse-grained material --> well-permeable gravel in valley bottoms
 3       hygeol_unconsol_medium_perc   unconsolidated medium-grained material --> permeable gravel outside of valley bottoms,
                                                                                  sandy gravel,medium- to coarse-grained gravel
 4,5     hygeol_unconsol_fine_perc     unconsolidated fine-grained material --> loamy gravel, fine- to medium-grained debris, moraines
 6       hygeol_unconsol_imperm_perc   impermeable, unconsolidated material --> clay, silt, fine sands and loamy moraines
 8       hygeol_karst_perc             karstic rock --> carbonate rock: limestone, dolomite, rauhwacke;
                                                         sulphate-containing rock: gypsum, anhydrite
 9,10    hygeol_hardrock_perc          hard rock --> fissured and porous, non-karstic hard rock: conglomerates,
                                                     sandstone, limestone with marl layers; crystalline rock: granite,
                                                     granodiorites, tonalite.
 11      hygeol_hardrock_imperm_perc   impermeable hard rock --> marl, shale, gneiss and cemented sandstone
 98,99   hygeol_water_perc             water --> glaciers, firn, surface waters
   
 Please refer also to table A1 in Appendix A for the corresponding classes in CAMELS-GB.
 
 4. For the area missing in Germany: download manually from [Hydrogeological Map of Germany 1:250'000](https://gdk.gdi-de.org/geonetwork/srv/api/records/61ac4628-6b62-48c6-89b8-46270819f0d6).
    For a procuct description see [prod. descr. Hydrogeo. Map DE](https://www.bgr.bund.de/DE/Themen/Wasser/Projekte/laufend/Beratung/Huek200/huek200_projektbeschr.html)
 5. Save data in Sys.getenv('CAMELS_DIR_DATA')/Hydrogeology/Karst/huek250_v103. You find the data you need in the subdirectory shp: shapefile huek250__25832_v103_poly.shp
 6. You are all done to run the code!

**Contributors**: Martina Kauzlaric, Ursula Schoenenberger, Daniel Viviroli

## Geology

**Source data**:
* ...

**Code used**: 
* ...

**Instructions**:
1. ...
2. ...

**Contributors**: Marius Floriancic


## Glacier

**Attributes description**:
* gauge_id: CAMELS-CH gauge ID
* glac_area: glacier area (km2) in Switzerland, evolution per catchment between 1980 and 2021
* glac_vol: glacier volume (km3) in Switzerland, evolution per catchment between 1980 and 2021
* glac_mass: glacier mass (mega tons) in Switzerland, calculated from glac_vol*850, evolution per catchment between 1980 and 2021
* glac_area_neighbours: glacier area (km2) in neighouring countries (France, Italy, Germany, Austria), evolution per catchment between 1980 and 2021

**Source data**:
* Glamos_1973 https://doi.glamos.ch/data/inventory/inventory_sgi1973_r1976.html
* Glamos_2016 https://doi.glamos.ch/data/inventory/inventory_sgi2016_r2020.html
* GLIMS_2015 glacier inventory Paul et al. (2020): Paul, F et al. (2019): Glacier inventory of the Alps from Sentinel-2, shape files (pangaea.de) https://doi.pangaea.de/10.1594/PANGAEA.909133
* GLIMS_2003 glacier inventory Paul et al. (2011): https://www.glims.org/maps/textsearch/
* evolution table from sig link from matthias.huss@unifr.ch
* evolution table from gi link from sibylle.wilhelm@giub.unibe.ch

**Code used**: 
* glacier_attributes/produce_glaciers_CH.R

**Instructions**:
1. Get shape files from Glamos and GLIMS -> Link
2. Get evolution tables from sgi and gi -> Link
3. Use code provided in camels_ch_glacier_aggregation.R to compute the different attributes

**Contributors**: Marvin Höge, Ursula Schoenenberger, Sibylle Wilhelm, Matthias Huss


## Land cover

**Source data**:
* clc 1990, clc 2000, clc 2006, clc 2012 and clc 2018 on copernicus server: https://land.copernicus.eu/pan-european/corine-land-cover

**Reclassification**:
* clc_code: 111, 	clc_LABEL3: Continuous urban fabric, 	camels_ch_attribute: urban
* clc_code: 112, 	clc_LABEL3: Discontinuous urban fabric, 	camels_ch_attribute: urban
* clc_code: 121, 	clc_LABEL3: Industrial or commercial units, 	camels_ch_attribute: urban
* clc_code: 122, 	clc_LABEL3: Road and rail networks and associated land, 	camels_ch_attribute: urban
* clc_code: 123, 	clc_LABEL3: Port areas, 	camels_ch_attribute: urban
* clc_code: 124, 	clc_LABEL3: Airports, 	camels_ch_attribute: urban
* clc_code: 131, 	clc_LABEL3: Mineral extraction sites, 	camels_ch_attribute: loose_rock
* clc_code: 132, 	clc_LABEL3: Dump sites, 	camels_ch_attribute: loose_rock
* clc_code: 133, 	clc_LABEL3: Construction sites, 	camels_ch_attribute: loose_rock
* clc_code: 141, 	clc_LABEL3: Green urban areas, 	camels_ch_attribute: grass
* clc_code: 142, 	clc_LABEL3: Sport and leisure facilities, 	camels_ch_attribute: urban
* clc_code: 211, 	clc_LABEL3: Non-irrigated arable land, 	camels_ch_attribute: crop
* clc_code: 212, 	clc_LABEL3: Permanently irrigated land, 	camels_ch_attribute: crop
* clc_code: 213, 	clc_LABEL3: Rice fields, 	camels_ch_attribute: crop
* clc_code: 221, 	clc_LABEL3: Vineyards, 	camels_ch_attribute: scrub
* clc_code: 222, 	clc_LABEL3: Fruit trees and berry plantations, 	camels_ch_attribute: scrub
* clc_code: 223, 	clc_LABEL3: Olive groves, 	camels_ch_attribute: scrub
* clc_code: 231, 	clc_LABEL3: Pastures, 	camels_ch_attribute: grass
* clc_code: 241, 	clc_LABEL3: Annual crops associated with permanent crops, 	camels_ch_attribute: crop
* clc_code: 242, 	clc_LABEL3: Complex cultivation patterns, 	camels_ch_attribute: crop
* clc_code: 243, 	clc_LABEL3: Land principally occupied by agriculture, with significant areas of natural vegetation, 	camels_ch_attribute: crop
* clc_code: 244, 	clc_LABEL3: Agro-forestry areas, 	camels_ch_attribute: scrub
* clc_code: 311, 	clc_LABEL3: Broad-leaved forest, 	camels_ch_attribute: dwood
* clc_code: 312, 	clc_LABEL3: Coniferous forest, 	camels_ch_attribute: ewood
* clc_code: 313, 	clc_LABEL3: Mixed forest, 	camels_ch_attribute: mixed_wood
* clc_code: 321, 	clc_LABEL3: Natural grasslands, 	camels_ch_attribute: grass
* clc_code: 322, 	clc_LABEL3: Moors and heathland, 	camels_ch_attribute: wetlands
* clc_code: 323, 	clc_LABEL3: Sclerophyllous vegetation, 	camels_ch_attribute: scrub
* clc_code: 324, 	clc_LABEL3: Transitional woodland-shrub, 	camels_ch_attribute: scrub
* clc_code: 331, 	clc_LABEL3: Beaches, dunes, sands, 	camels_ch_attribute: loose_rock
* clc_code: 332, 	clc_LABEL3: Bare rocks, 	camels_ch_attribute: rock
* clc_code: 333, 	clc_LABEL3: Sparsely vegetated areas, 	camels_ch_attribute: loose_rock
* clc_code: 334, 	clc_LABEL3: Burnt areas, 	camels_ch_attribute: loose_rock
* clc_code: 335, 	clc_LABEL3: Glaciers and perpetual snow, 	camels_ch_attribute: ice
* clc_code: 411, 	clc_LABEL3: Inland marshes, 	camels_ch_attribute: wetlands
* clc_code: 412, 	clc_LABEL3: Peat bogs, 	camels_ch_attribute: wetlands
* clc_code: 421, 	clc_LABEL3: Salt marshes, 	camels_ch_attribute: wetlands
* clc_code: 422, 	clc_LABEL3: Salines, 	camels_ch_attribute: wetlands
* clc_code: 423, 	clc_LABEL3: Intertidal flats, 	camels_ch_attribute: wetlands
* clc_code: 511, 	clc_LABEL3: Water courses, 	camels_ch_attribute: inwater
* clc_code: 512, 	clc_LABEL3: Water bodies, 	camels_ch_attribute: inwater
* clc_code: 521, 	clc_LABEL3: Coastal lagoons, 	camels_ch_attribute: inwater
* clc_code: 522, 	clc_LABEL3: Estuaries, 	camels_ch_attribute: inwater
* clc_code: 523, 	clc_LABEL3: Sea and ocean, 	camels_ch_attribute: inwater
* clc_code: 999, 	clc_LABEL3: NODATA, 	camels_ch_attribute: inwater
* clc_code: 990, 	clc_LABEL3: UNCLASSIFIED LAND SURFACE, 	camels_ch_attribute: inwater
* clc_code: 995, 	clc_LABEL3: UNCLASSIFIED WATER BODIES, 	camels_ch_attribute: inwater

**Code used**: 
* landcover_attributes/corine_landcover_CH.R
* landcover_attributes/annual_timeserie_CH.R

**Instructions**:
1. Get clc data from copernicus server
2. Use code provided in camels_ch_clc.R to compute the different attributes
3. Note: clc_1990 has no data for Switzerland, so catchments with 5% or more missing data were filled with NaN.

**Contributors**: Ursula Schoenenberger, Jan Schwanbeck

## Human impact

**Attributes description**: 
* gauge_id: CAMELS-CH gauge ID
* n_inhabitants: number of inhabitants
* dens_inhabitants: density of inhabitants
* hp_count: 
* hp_ qturb:
* hp_inst_turb:
* hp_max_power: 
* num_reservoir: number of reservoirs in catchment
* reservoir_cap: total reservoir storage capacity within a catchment in ML
* reservoir_he: percentage of total reservoir storage in catchment used for hydroelectricty production
* reservoir_fs: percentage of total reservoir storage in catchment used for flood storage
* reservoir_irr: percentage of total reservoir storage in catchment for irrigation
* reservoir_nousedata: percentage of total reservoir storage where no use data were available
* reservoir_year_first: year the first reservoir in the catchment was built
* reservoir_year_last: year the last reservoir in the catchment was built 

**Source data**:
* Reservoir information from the Swiss Federal Office of Energy SFOE. For more details see: https://www.bfe.admin.ch/bfe/de/home/versorgung/statistik-und-geodaten/geoinformation/geodaten/wasser/stauanlagen-unter-bundesaufsicht.html. 

**Code used**: 
* human_impact_attributes/produce_reservoirs_CH.R

**Instructions**:
1. Get reservoir data from SFOE
2. Use code provided in camels_ch_reservoirs.R to compute the different attributes
3. Detailed descriptions of how the different attributes are computed are provided in camels_ch_reservoirs.R

**Contributors**: Manuela Brunner, Marvin Höge
