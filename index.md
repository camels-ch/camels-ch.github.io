# CAMELS-CH reproducibility guide

The purpose of this guide is to describe the creation of the CAMELS-CH dataset for repoducibility and to faciliate future updating and customized application. Corresponding codes are generally available at https://github.com/camels-ch/camels and specified in each attribute category below.

## Table of Contents

<!-- TOC start -->
- [Conventions on file content](#conventions-on-file-content)
- [CAMELS_CH_topographic_attributes](#camels_ch_topographic_attributes)
  * [Gauge data](#gauge-data)
  * [Catchment properties](#catchment-properties)
- [CAMELS_CH_climatic_attributes](#camels_ch_climatic_attributes)
- [CAMELS_CH_hydrologic_attributes](#camels_ch_hydrologic_attributes)
- [CAMELS_CH_landcover_attributes](#camels_ch_landcover_attributes)
- [CAMELS_CH_soil_attributes](#camels_ch_soil_attributes)
- [CAMELS_CH_hydrogeology_attributes](#camels_ch_hydrogeology_attributes)
- [CAMELS_CH_geology_attributes](#camels_ch_geology_attributes)
- [CAMELS_CH_humaninfluence_attributes](#camels_ch_humaninfluence_attributes)
- [CAMELS_CH_glaciers_attributes](#camels_ch_glaciers_attributes)
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

## CAMELS_CH_topographic_attributes

### Gauge data

**Attributes**: gauge_id, gauge_name, gauge_lat, gauge_lon, gauge_easting, gauge_northing, gauge_elev

**Source data**:
* Main data source: BAFU gauge properties
* Complemented with the EU-Hydro river network database (https://land.copernicus.eu/imagery-in-situ/eu-hydro)

**Instructions**:
1. ...
2. ...

**Contributors**: ...

### Catchment properties

**Attributes**: area, dpsbar, elev_mean, elev_min, elev_10, elev_50, elev_90, elev_max

**Source data**:
* Main data source: Catchement shapefiles provided by BAFU
* Complemented with the EU-Hydro river network database (https://land.copernicus.eu/imagery-in-situ/eu-hydro)

**Code used**: ...

**Instructions**:
1. ...
2. ...

**Contributors**: ...

## CAMELS_CH_climatic_attributes
**Attributes**: "gauge_ID", "sign_start_date", "sign_end_date", "sign_number_of_years", "p_mean", "pet_mean", "aridity", "p_seasonality", "frac_snow", "high_prec_freq" ,"high_prec_dur", "high_prec_timing", "low_prec_freq", "low_prec_dur", "low_prec_timing" 

**Versions**: Climatic indices are calculated based on i) observed data, and ii) simulated data.

**Source data**:
* Observed data: Temperature and precipitation time series are from MeteoSwiss, and potential evapotranspiration time series are from Prevah (and thus simulated). The observation-based climatic indices were calculated for the same time period as the observation-based hydrological signatures to ease comparison.
* Simulated data: Streamflow time series are from Prevah simulations. The simulation-based climatic indices were calculated for the same time period as the simulation-based hydrological signatures to ease comparison.

**Code used**: https://github.com/camels-ch/camels/blob/ch-specific/compute/climate_indices.R

**Instructions**:
* Note: Only years with complete hydrological year are used (5% missing values are tolerated per hydrological year).
* Note: The variables "high_prec_timing" and "low_prec_timing" can have NA values if the number of high precipitation days or the number of low precipitation days is identical in multiple seasons.

**Contributors**: ...

## CAMELS_CH_hydrologic_attributes

**Attributes**: "gauge_ID","sign_start_date","sign_end_date","sign_number_of_years","q_mean","runoff_ratio","stream_elas","slope_fdc","baseflow_index_landson" "hfd_mean","Q5","Q95","high_q_freq","high_q_dur","low_q_freq","low_q_dur","zero_q_freq"

**Versions**: Hydrological signatures are calculated based on i) observed data, and ii) simulated data.

**Source data**:
* Observed data: Streamflow time series provided by BAFU. The length of the observed time series varies among catchments.
* Simulated data: Streamflow time series are from Prevah simulations. The simulated time series is identical for all catchments and lasts from 1981-10-01 to 2020-09-30.

**Code used**: https://github.com/camels-ch/camels/blob/ch-specific/compute/hydro_signatures.R

**Instructions**:
* Note: Only years with complete hydrological year are used (5% missing values are tolerated per hydrological year).
* Note: The variable "stream_elas" can have a value of NA if only one complete hydrological year is available.

**Contributors**: ...


## CAMELS_CH_soil_attributes


## CAMELS_CH_hydrogeology_attributes

**Attributes**: gauge_id, hygeol_unconsol_coarse_perc, hygeol_unconsol_medium_perc, hygeol_unconsol_fine_perc, hygeol_unconsol_imperm_perc, hygeol_karst_perc, hygeol_hardrock_perc, hygeol_hardrock_imperm_perc, hygeol_water_perc, hygeol_external_perc

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

**Instructions**:
1. Get source data from data.geo.admin.ch (last accessed 18.03.2022)
2. 

**Contributors**: Daniel Viviroli, Ursula Schoenenberger, Martina Kauzlaric

## CAMELS_CH_geology_attributes


## CAMELS_CH_glacier_attributes

**Attributes**: gauge_id, glac_area, glac_vol, glac_mass, glac_area_neighbours

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

**Code used**: extract/camels_ch_glacier.R

**Instructions**:
1. Get shape files from Glamos and GLIMS -> Link
2. Get evolution tables from sgi and gi -> Link
3. Use code provided in camels_ch_glacier_aggregation.R to compute the different attributes

**Contributors**: Marvin Höge, Ursula Schoenenberger, Sibylle Wilhelm, Matthias Huss



## CAMELS_CH_landcover_attributes

**Attributes**:gauge_id,crop_perc,ewood_perc,dwood_perc,grass_perc,ice_perc,inwater_perc,loose_rock_perc,mix_wood_perc,rock_perc,shrub_perc,urban_perc,wetlands_perc,blank_perc 

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

**Code used**: landcover_attributes/camels_ch_clc.R and landcover_attributes/camels_ch_annual_timeserie.R

**Instructions**:
1. Get clc data from copernicus server
2. Use code provided in camels_ch_clc.R to compute the different attributes
3. Note: clc_1990 has no data for Switzerland, so catchments with 5% or more missing data were filled with NaN.

**Contributors**: Ursula Schoenenberger, Jan Schwanbeck

## CAMELS_CH_humaninfluence_attributes

**Attributes**: gauge_id, n_inhabitants, dens_inhabitants, hp_count, hp_qturb, hp_inst_turb, hp_max_power, num_reservoir, reservoir_cap, reservoir_he, reservoir_fs, reservoir_irr, reservoir_nousedata, reservoir_year_first, reservoir_year_last

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

**Code used**: extract/camels_ch_reservoirs.R

**Instructions**:
1. Get reservoir data from SFOE
2. Use code provided in camels_ch_reservoirs.R to compute the different attributes
3. Detailed descriptions of how the different attributes are computed are provided in camels_ch_reservoirs.R

**Contributors**: Manuela Brunner, Marvin Höge
