# CAMELS-CH reproducibility guide

The purpose of this guide is to describe the creation of the CAMELS-CH dataset in order to facilitate its future updating.

## Table of Contents

<!-- TOC start -->
- [Conventions on file content](#conventions-on-file-content)
- [CAMELS_CH_topographic_attributes](#camels_ch_topographic_attributes)
  * [Gauge data](#gauge-data)
  * [Catchment properties](#catchment-properties)
- [CAMELS_CH_climatic_attributes](#camels_ch_climatic_attributes)
- [CAMELS_CH_hydrologic_attributes](#camels_ch_hydrologic_attributes)
  * [Hydrologic Signatures](#hydrologic-signatures)
- [CAMELS_CH_landcover_attributes](#camels_ch_landcover_attributes)
- [CAMELS_CH_soil_attributes](#camels_ch_soil_attributes)
- [CAMELS_CH_hydrogeology_attributes](#camels_ch_hydrogeology_attributes)
- [CAMELS_CH_hydrometry_attributes](#camels_ch_hydrometry_attributes)
- [CAMELS_CH_humaninfluence_attributes](#camels_ch_humaninfluence_attributes)
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

### Catchment properties

**Attributes**: area, dpsbar, elev_mean, elev_min, elev_10, elev_50, elev_90, elev_max

**Source data**:
* Main data source: Catchement shapefiles provided by BAFU
* Complemented with the EU-Hydro river network database (https://land.copernicus.eu/imagery-in-situ/eu-hydro)

**Code used**: ...

**Instructions**:
1. ...
2. ...


## CAMELS_CH_climatic_attributes


## CAMELS_CH_hydrologic_attributes

### Hydrologic Signatures

**Attributes**: q_mean, runoff_ratio, stream_elas, slope_fdc, baseflow_index, baseflow_index_ceh, hfd_mean, Q5, Q95, high_q_freq, high_q_dur, low_q_freq, low_q_dur, zero_q_freq

**Source data**:
* Streamflow time series provided by BAFU

**Code used**: hydro/hydro_signatures.R

**Instructions**:
1. ...
2. ...


## CAMELS_CH_landcover_attributes

**Attributes**:crop_perc,ewood_perc,dwood_perc,grass_perc,ice_perc,inwater_perc,loose_rock_perc,mix_wood_perc,rock_perc,shrub_perc,urban_perc,wetlands_perc,blank_perc 

**Source data**:
* clc copernicus server: 1990, 2000, 2006, 2012 and 2018

**Code used**: extract/camels_ch_clc.R

## CAMELS_CH_soil_attributes


## CAMELS_CH_hydrogeology_attributes

**Attributes**: hygeol_unconsol_coarse_perc, hygeol_unconsol_medium_perc, hygeol_unconsol_fine_perc, hygeol_unconsol_imperm_perc, hygeol_karst_perc, hygeol_hardrock_perc, hygeol_hardrock_imperm_perc, hygeol_water_perc, hygeol_external_perc

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

**Code used**: extract/hydrogeol_reclass.py, extract/camels_ch_hydrogeo.R

**Instructions**:
1. Get source data from data.geo.admin.ch (last accessed 18.03.2022)
2. From source data use layer "PY_Basis_Flaechen" and there field "H2_ID"
3. Open ArcMap field calculator. Chose Python as parser and activate "show codeblock"
4. Paste contents of hydrogeol_reclass.py into field calculator's pre-logic script code window (use this for the numeric field ID1)
5. Fill in field calculator's expression box (for both ID1 and ID2) with "calc(!H2_ID!)" (without quote marks)
6. Hit "Compute" in field calculator


## CAMELS_CH_hydrometry_attributes


## CAMELS_CH_humaninfluence_attributes

**Attributes**: num_reservoir, reservoir_cap, reservoir_he, reservoir_fs, reservoir_irr, reservoir_nousedata, reservoir_year_first, reservoir_year_last

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

