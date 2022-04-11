# lakeCoSTR User Guide

We created lakeCoSTR to make the acquisition and analysis of Landsat's Collection 2 Surface Temperature product more accessible for those interested in obtaining historical estimates of lake temperature from remote sensing data. This tool is geared towards researchers interested in gathering historical temperature estimates, especially for small lakes, who don't necessarily have experience working with remote sensing data. This tool leverages the unprecedented historical Landsat 4, 5, 7, and 8 Collection 2 remote sensing dataset using Google Earth Engine (GEE) (Gorelick, et al. 2017), the GEE Python API, and Google Colaboratory (Bisong, 2019).

You must have <a href="https://support.google.com/accounts/answer/27441?hl=en" target="_blank" rel="noopener noreferrer">Google Account</a> and <a href="https://signup.earthengine.google.com/" target="_blank" rel="noopener noreferrer">Google Earth Engine Credentials</a> in order to use this tool.

lakeCoSTR is an open-source tool, but please cite accordingly - the widget provided on the right side of the <a href = "https://github.com/lakeCoSTR/lakeCoSTR_colab" target="_blank" rel="noopener noreferrer">repository landing page</a> will provide a citation.


## Features of lakeCoSTR

__lakeCoSTR aggregates the surface temperature estimates into summary values per Landsat scene and outputs those data in a single *.csv* file for the requested time of interest__
 - The tool provides summary statistics for each scene, including mean estimated temperature, five-number summary of the estimated temperature, skew, and distrubution kurtosis
 - The tool also provides histograms of each Landsat scene for users to visually examine the spread and distribution of pixel-level data to assist the user in choosing a meaningful summary statistic and aid in further data filtering

__lakeCoSTR makes it easy to pair the Landsat surface temperature estimates with user-provided *in situ* temperature data__
 - We HIGHLY RECCOMMEND that users create a Landsat-*in situ* dataset at this time prior to applying any Landsat-derived temperature estimate to their ecological question, as large-scale evaluation of Collection 2 temperature estimates for lakes has not yet been completed. For information about current validation practices of Collection 2 data by NASA, please see Cook 2014.


## Constraints and Caveats 

__lakeCoSTR does not correct for the impact of cloud and cloud shadows__
 - Cook (2014) identified that the Landsat surface temperature product was affected by cloud and cloud shadows, creating pixel-level estimates that underestimated actual surface temperature of the water
 - preliminary analyses using lakeCoSTR to estimate lake surface temperature for Lake Sunapee in New Hampshire, USA showed that no calibration to *in situ* data was needed for the use of Landsat temperature estimates and that most perceived atmospheric contamination was eliminated using a filter on the kurtosis of the histogram of pixel-level temperature data (Herrick and Steele et al., currently in peer review) 

__lakeCoSTR works best for small lakes__
 - small lakes are usually completely contained within one or more <a href = "https://landsat.gsfc.nasa.gov/about/the-worldwide-reference-system/" target="_blank" rel="noopener noreferrer">WRS-2 path rows</a>. lakeCoSTR does not aggregate data between multiple Landsat path-rows. Users will be messaged in the tool if there are issues with this
 - small lakes require a smaller amount of cloud compute power and are less likely to time-out the Google Earth Engine API within the tool. If a compute time-out occurs, users will be notified of how to troubleshoot within the tool
 - lake-of-interest must have a surface area of at least 0.4ha or approximately 42 30x30m pixels large for lakeCoSTR to provide meaningful summary statistics.

__lakeCoSTR works best for non-connected lakes__
 - the automatic delineation tool used in lakeCoSTR will include streams and connected waterbodies within the user-drawn bounding box if they are connected by a water pixels visible by Landsat's 30x30m pixel size. If you are interested in using lakeCoSTR for an area smaller than is automatically delineated by the tool, you may upload a shapefile or vector file to override the automatic delineation.

__lakeCoSTR was only designed to extract surface temperature for lakes during ice-free periods__
 - all pixels that have been labeled as 'ice' or 'snow' will be filtered from the scene if present and flagged in the Bit Quality Assessment (BQA) band of the scene metadata

__lakeCoSTR may not perform well at high latitudes__
 - there is currently no solar zenith angle cutoff employed in the lakeCoSTR tool: this may result in distortions of temperature estimates and location precision
 - solar zenith angle, as provided in the Landsat scene metadata is included in the output *.csv*

__lake-specific Landsat-*in situ* comparisons are highly encouraged__
 - the Landsat surface temperature product used in lakeCoSTR has not been widely tested across the world - comparisons between the Landsat surface temperature estiamtes and *in situ* data at a lake-by-lake scale are encouraged at this time


## Using lakeCoSTR

### If you've never used Colab:

First, visit the <a href = "https://colab.research.google.com/" target="_blank" rel="noopener noreferrer">Colab Basics</a> webpage and run through the script that pops up on this page. It will walk your through how to use a Colab Notebook and run cells.

### Open the tool 

There are two primary ways to open the lakeCoSTR tool in Google Colaboratory:

#### 1) if you have a GitHub account:
    
a) simply fork the lakeCoSTR_colab repository, navigate to the lakeCoSTR.ipynb notebook and click the 'Open in Colab' link at the top of the file (circled below in RED)
    
   ![Open in Colab](imgs/lakeCoSTR_open.png)

b) In the Colab browser, click 'File' -> 'Save a copy in Drive' - this will save a copy of the lakeCoSTR tool to your Google Drive in a folder called 'Colab Notebooks'.
    
   ![Save to Drive](imgs/lakeCoSTR_save.png)


#### 2) if you __DO NOT__ have a GitHub account:
    
a) on the landing page of the repository, click the green button labeled 'Code' and choose the option 'Download ZIP'. This will download the reopository to your computer.
    
   ![Download the Repo](imgs/lakeCoSTR_download.png)

b) unzip the folder

c) navigate to <a href = "https://colab.research.google.com/" target="_blank" rel="noopener noreferrer">the Colab browser</a>

d) In the Colab browser, upload the lakeCoSTR.ipynb file from the downloaded repository by clicking 'File' -> 'Upload Notebook':
    
   ![Upload file](imgs/lakeCoSTR_upload.png)

e) In the Colab browser, click 'File' -> 'Save a copy in Drive' - this will save a copy of the lakeCoSTR tool to your Google Drive in a folder called 'Colab Notebooks'.
    
   ![Save to Drive](imgs/lakeCoSTR_save.png)

### Run through the lakeCoSTR script

This should be pretty easy - lots of clicking that run ![colab run](imgs/colab_run.png) button! 

If you come across an issue with lakeCoSTR, please save your Colab notebook, screenshot your error, and send us an email (steeleb@caryinstitute.org and christina.herrick@unh.edu) or submit an issue on GitHub. 

#### details about *in situ* data formatting

In order to pair the acquired Landsat data with *in situ* data, you'll need to format the data in a specific manner and tell us a few things about the timestamp of the data provided. An input data example is available at the [lakeCoSTR_examples](https://github.com/lakeCoSTR/lakeCoSTR_examples) repository.

1) Your *in situ* file must be a .csv and have the following column names:

        datetime: Date and time of each temperature reading in a recognized *strftime* format

        temp_degC: an integer or float number representing temperature in degrees Celcius

        location: for in-lake statistics, a column with lake zone names (string format, no special characters); can be all the same for a single output or differentiated by lake zones/sensors
        
        depth_m: as an integer or float number, for statistics about the depth of sensors for matched scenes

2) your <datetime> column must be in <a href="https://strftime.org/" target = "_blank" rel="noopener noreferrer">strftime</a> format. In order for lakeCoSTR to provide you with a Landsat-*in situ* match file, you will have to indicate what the format of the datetime column is. 

    - for example, if your date is formatted like this: '2007-10-05 18:07:00', your strftime format is: '%Y-%m-%d %H:%M:%S'. 

    - or: '05Oct2007 6:07 PM', your strftime format is: '%d%b%Y %I:%M %p'
   
3) you must indicate an <a href="https://en.wikipedia.org/wiki/List_of_tz_database_time_zones" target = "_blank" rel = "noopener noreferrer">Olson Database Time Zone</a>. 

    - the <insitu_timezone> requires you to enter the text from the __TZ database name__ EXACTLY of the corresponding row of the above-linked table. 

    - please pay special attention to the STD (standard time) and DST (daylight savings time) offsets to make sure they align with your data. 

    - if you use the *Etc/GMT* option, where no DST is observed, please note that the offset signs are *intentionally inverted* 

#### output file definitions and documentation

Examples of data output are also found at the [lakeCoSTR_examples](https://github.com/lakeCoSTR/lakeCoSTR_examples) repository.

__GoogleDrive/lakeCoSTR_output/*LAKENAME*/*LAKENAME*_temp_stats.csv__

This is the *.csv* represents the summary data for every available Landsat 4-8 scene, given the user-defined lake area and time period of interest. This output file from lakeCoSTR has the same format no matter the lake or time period extent.

| Column Header | Column definition | Units | Source of data | 
|   --- | --- | --- | --- |
|   system:index    |   Landsat scene identifier: *PROCESSINGLEVEL*_*LANDSATCOLLECTION*_*LANDSATDATATIER*_*LANDSATMISSION*_*PATHROW*_*ACQUISITIONDATE* | textString | scene-level metadata |
|	availablePixels_count | total pixels available for analysis, after pixel-level bit mask and geometry masks applied | pixel | GSWD masked by bit QA and geometry of lake |
|	cloudcover_pct_scene | percent cloud cover of entire Landsat scene | percent | Landsat scene metadata |
|	datetime_landsat | datetime stamp of image acquisition in Landsat datetime format | integer; milliseconds since epoch | Landsat scene metadata |
|	esd_au_scene | earth-sun distance | astronomical units | Landsat scene metadata |
|	lakeCoverage_pct | percent of coverage of the lake in a given scene, calculated as (availablePixels_count/total number of GSWD pixels) * 100 | percent | GSWD masked by bit QA and geometry of lake |
|	sunazi_deg_scene | sun azimuth angle | degree | Landsat scene metadata |
|	sunelev_deg_scene | sun elevation | degree | Landsat scene metadata |
|	surface_temp_kurtosis | measure of tailedness of histogram for temperature estimates of the lake for a given scene | none | pixel-level analysis |
|	surface_temp_max | maximum temperature for a given scene | degreeCelsius | pixel-level analysis | 
|	surface_temp_mean |  mean temperature for a given scene | degreeCelsius | pixel-level analysis | 
|	surface_temp_median |  median temperature for a given scene | degreeCelsius | pixel-level analysis | 
|	surface_temp_min |  minimum temperature for a given scene | degreeCelsius | pixel-level analysis | 
|	surface_temp_p25 |  25th percentile temperature for a given scene | degreeCelsius | pixel-level analysis | 
|	surface_temp_p75 |  75th percentile temperature for a given scene | degreeCelsius | pixel-level analysis | 
|	surface_temp_skew | skew of the histogram for temperature estimates of the lake for a given scene | none | pixel-level analysis |
|	surface_temp_stdDev | standard deviation from the mean for the temeperature estimates of the lake for a given scene | degreeCelsius | pixel-level analysis |
|   .geo |  artifact of GEE extract |   none |  GEE |

__GoogleDrive/lakeCoSTR_output/*LAKENAME*/histograms__

This folder contains the exported .png files of the histograms of the pixel-level temperature estimates. 

The file names are formatted as the following:

*MISSION*_*PROCESSINGLEVEL*_*PATHROW*_*ACQUISITIONDATE*_*PROCESSINGDATE*_*LANDSATCOLLECTION*_*LANDSATTIER*_histo.png

An example is:
*LC08*_*L1TP*_*013030*_*20150702*_*20200909*_*02*_*T1*_histo

This would be the histogram for the *Landsat 8* mission, *Level 1* processing, path *013* row *030*, acquired ong *2015-07-02*, processed on *2020-09-09*, *Collection 2*, *Tier 1*.

__GoogleDrive/lakeCoSTR_output/*LAKENAME*/*LAKENAME*_temp_landsat_paired.csv__

This is the *.csv* that is the result of using the Landsat-*in situ* pairing process in lakeCoSTR. Here, the primary output file (*LAKENAME*_temp_stats.csv) is filtered and matched with user-provided *in situ* data as described above. The paired dataset copies the columns from the *LAKENAME*_temp_stats.csv and adds a few additional columns, where *LOCATION* is gathered from the *in situ* data column 'location'. When multiple locations are present in the user-provided dataset, the median, mean, standard deviation and count *LOCATION* columns will be repeated with a new location name.

| Column Header | Column definition | Units | Source of data | 
|   --- | --- | --- | --- |
|   landsat_time_utc    | datetime of the scene acquistion in UTC time |    YYYY-MM-DD HH:MM:SS |  Landsat scene metadata |
|	scene | Landsat scene identifier: *MISSION*_*PROCESSINGLEVEL*_*PATHROW*_*ACQUISITIONDATE*_*PROCESSINGDATE*_*LANDSATCOLLECTION*_*LANDSATTIER* | textString | Landsat scene metadata |
|	is_temp_avg | average *in situ* temperature measured in user-defined time window, across all locations | degreeCelsius | user-provided *in situ* data file |
|	is_temp_stdev | standard deviation of the mean for *in situ* temperature measured in user-defined time window, across all locations | degreeCelsius | user-provided *in situ* data file |
|	is_depth_avg | average depth of *in situ* values in user-defined time window, across all locations | meter | user-provided *in situ* data file |
|	is_depth_stdev | standard deviation of the mean for the depth of *in situ* values in user-defined time window, across all locations | meter | user-provided *in situ* data file |
|	is_temp_med | median *in situ* temperature measured in user-defined time window, across all locations | degreeCelsius | user-provided *in situ* data file |
|	insitu_count | total number of *in situ* measurements contributing to the aggregated values | count | user-provided *in situ* data file |
|	*LOCATION*_median | median *in situ* temperature measured in user-defined time window, at *LOCATION* | degreeCelsius | user-provided *in situ* data file |
|	*LOCATION*_mean | average *in situ* temperature measured in user-defined time window, at *LOCATION* | degreeCelsius | user-provided *in situ* data file |
|	*LOCATION*_std | standard deviation of the mean *in situ* temperature measured in user-defined time window, at *LOCATION* | degreeCelsius | user-provided *in situ* data file |
|	*LOCATION*_count| total number of *in situ* measurements contributing to the aggregated values at *LOCATION* | count | user-provided *in situ* data file |

__GoogleDrive/lakeCoSTR_output/*LAKENAME*/ancillary__

This folder contains individual *.csv* files for each scene, listing the individual temperature measurements included in the aggregated data found in the *LAKENAME*_temp_landsat_paired.csv file. File names are labeled with the 'scene' value from the *LAKENAME*_temp_landsat_paired.csv file. We encourage users to check this file to make sure that they have selected the correct UTC offset in the lakeCoSTR tool. This file copies all columns from the user-provided *in situ* data file and then adds the following additional columns:

| Column Header | Column definition | Units | Source of data | 
|   --- | --- | --- | --- |
|	datetime_utc | datetime, converted to UTC time based on the user-defined timezone | YYYY-MM-DD HH:MM | user-provided *in situ* data file |
|	dt_delta |  difference between datetime_utc (*in situ* measurement) and Landsat acquisition time (landsat_time_utc in *LAKENAME*_temp_landsat_paired.csv file) | DAYS HH:MM:SS | lakeCoSTR calculation |
|	delta_secs | difference between datetime_utc (*in situ* measurement) and Landsat acquisition time (landsat_time_utc in *LAKENAME*_temp_landsat_paired.csv file) in seconds | second | lakeCoSTR calculation |



## *Citations:*

Bisong, Ekaba. 2019. “Google Colaboratory.” In Building Machine Learning and Deep Learning Models on Google Cloud Platform: A Comprehensive Guide for Beginners, edited by Ekaba Bisong, 59–64. Berkeley, CA: Apress. https://doi.org/10.1007/978-1-4842-4470-8_7.

Cook, M. (2014). Atmospheric Compensation for a Landsat Land Surface Temperature Product. Thesis. Rochester Institute of Technology. Accessed from http://scholarworks.rit.edu/theses/8513.

Gorelick, Noel, Matt Hancher, Mike Dixon, Simon Ilyushchenko, David Thau, and Rebecca Moore. 2017. “Google Earth Engine: Planetary-Scale Geospatial Analysis for Everyone.” Remote Sensing of Environment, Big Remotely Sensed Data: tools, applications and experiences, 202 (December): 18–27. https://doi.org/10.1016/j.rse.2017.06.031.

