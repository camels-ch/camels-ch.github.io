# CAMELS-CH reproducibility guide

The purpose of this guide is to describe the creation of the CAMELS-CH dataset in order to facilitate its future updating.


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


## CAMELS_CH_soil_attributes


## CAMELS_CH_hydrogeology_attributes


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

