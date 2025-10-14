***Latest version is v20251013***

# NFDRS_FEMS

ArcGIS Pro tool that automates the download and processing of historical FEMS data for user-specified RAWS stations. For each selected fire danger index, it creates:

- Daily listings
- Day of Year Summaries
- Percentile breakpoint tables  
- Percentile lookup tables 
- NFDRS charts

Products are generated at three geographic scales:

- **GACC** (Geographic Area Coordination Center)  
- **PSA** (Predictive Service Area)  
- **Station**
<br>

## Inputs

![screenshot_NFDRS_FEMS_1.png](/docs/screenshot_NFDRS_FEMS_1.png)

### Process Historical Data
If requested, FEMS data will be downloaded for all stations in the StationList.csv, and processed to generate daily listings, day of year summaries, percentile breakpoints, and percentile lookup tables.

### Create Charts
If requested, charts are created for each of the user's specified indices. The user must also specify if the FEMS "Production" or "Staging" environment should be used for downloading the current year and forecast data.

When requesting charts, the historical data only needs to be downloaded and processed a single time. Users may generate charts at any time by specifying an output directory still containing the previously downloaded and processed historical data.

The visual style of the charts was inspired by those made available via [Eric Drewitz's FireWxPy Python Library](https://pypi.org/project/firewxpy/)

![screenshot_NFDRS_FEMS_2.png](/docs/screenshot_NFDRS_FEMS_2.png)

### Select Indices to Process and Percentile Breakpoints
Specify one or more fire danger indices to process. The selected indices will be present in the output CSVs and will also have charts generated (if requested). Charts are available for 1000HrFM, 100HrFM, 10HrFM, 1HrFM, BI, ERC, IC, KBDI, and SC. The data analysis functions and chart creation adjusts dynamically based on the user's specified percentile breakpoints for each index.

### StationList CSV
Users must provide the [StationList.csv](https://github.com/mpanunto/NFDRS_FEMS/blob/main/StationList.csv) file as input, which specifies **1)** the stations to include in the analysis, **2)** which fuel models to use, **3)** period of record start/end dates, and **4)** percentile filter start/end dates. The CSV also provides the GACC and PSA for each station, which is critical for correctly averaging the data across stations to generate accurate GACC/PSA-level outputs. This CSV is included in the repository download, and is pre-populated with default stations from the Northern California Geographic Area Coordination Center (ONCC). Users may freely edit this file to include any stations they wish to process.

While the tool was originally designed to process FEMS data at the GACC and PSA levels, these fields of the input CSV are flexible, and can simply be considered as "major" and "minor" station groups. The only requirement is that users maintain consistent naming for their GACC and PSA values. For example, a PSA could represent a custom group of stations within a FDRA, Dispatch Boundary, or any other user-defined region. ***However, the original field names of the input CSV must not be changed***.

The below screenshot is an example of how the input CSV could be modified to download and process data for three FDRAs of the UTNUC (Northern Utah) dispatch boundary:

![screenshot_NFDRS_FEMS_1.png](/docs/screenshot_NFDRS_FEMS_3.png)

### Specify Output Directory
Specify directory to place all downloads and output products.
<br>
<br>

## Analysis Methods

The tool processes the FEMS data at 3 levels: GACC, PSA, and Station

### GACC
1. FEMS historical record downloaded for all stations in GACC
   - Downloads daily “extremes”, not hourly
   - “Extremes” because it is max() for all indices except 1HrFM/10HrFM/100HrFM/1000HrFM, which are min()
   - Leap years are adjusted.
     - Leap Days (02/29) are given a date of 02/28
     - In a leap year, each of the days beyond 02/29 have their “Day of Year” value (1-366) reduced by 1 to ensure the dates align with non-leap years.
2. Average daily extreme is then calculated for each index, for all days of historical record. This is done by averaging the daily extreme values across all stations for each day of historical record
   - This output is known as the ***“Daily Listing”***
3. Using the Daily Listing, the min/max/avg is then calculated for all indices for each “Day of Year” (1-365).
   - Ex: from 01-01-2005 to 12-31-2022, there are 18 July 1st’s in the daily listing. The min/max/avg of these 18 values is calculated to determine the “Day Of Year” min/max/avg for each index.
   - Some refer to the 1-365 values as the “Julian Day” values, but “Day of Year” is more appropriate.
4. A percentile filter (fire season date range filter) is then applied to the daily listing, which after being filtered is used to determine the percentile breakpoints, and percentile lookup tables for each index
   

### PSA:
 - Methods for GACC are applied to only the group of stations within each PSA
 - Repeated for each PSA

### Station
 - Methods for GACC are applied to each individual station. However, since it is just an individual station, the historical record IS the “Daily Listing”. No averaging across stations is needed.
 - Repeated for each Station
<br>

## Outputs
At the root of the output directory will be a "PctlBreakpoints_All.csv" and "PctlTable_All.csv", which provide percentile breakpoint and percentile lookup tables for all GACC/PSA/Stations.

Additionally, individual tabular outputs for each GACC/PSA/Station can be found by navigating to the GACC and/or PSA sub-folders within the "Historical" directory.
- xxxx_DailyExtremes.csv = The "Daily Listing" for the Station
- xxxx_DailyExtremes_PctlFilter.csv = The "Daily Listing" for the Station with percentile filter applied
- xxxx_AvgDailyExtremes.csv = The "Daily Listing" for the GACC/PSA
- xxxx_AvgDailyExtremes_PctlFilter.csv = The "Daily Listing" for the GACC/PSA with percentile filter applied
- xxxx_DOY.csv = The "Day of Year" min/25th/50th/avg/75th/max for the GACC/PSA/Station
- xxxx_PctlBreakpoints.csv = The percentile breakpoints for the GACC/PSA/Station
- xxxx_PctlTable.csv = The percentile lookup table for the GACC/PSA/Station

If charts were requested, they can be found in the "Charts" folder within each GACC directory.
<br>

## Usage

To use this toolbox:

1. [Download the repository](https://github.com/mpanunto/NFDRS_FEMS/archive/refs/heads/main.zip)
2. Extract NFDRS_FEMS.tbx
3. Run tool using ArcGIS Pro
