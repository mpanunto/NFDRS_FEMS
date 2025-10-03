***Latest version is v20250930***

# NFDRS_FEMS

Tool that automates the download and processing of historical FEMS data for user-specified RAWS stations. For each selected fire danger index, it creates:

- Daily listings  
- Percentile breakpoint tables  
- Percentile lookup tables  
- NFDRS charts

Products are generated at three geographic scales:

- **GACC** (Geographic Area Coordination Center)  
- **PSA** (Predictive Service Area)  
- **Station**

## Input Requirements

Users must provide the [StationList.csv](https://github.com/mpanunto/NFDRS_FEMS/blob/main/StationList.csv) file as input, which specifies the stations to include in the analysis and which fuel model to use for each. The CSV also provides the corresponding GACC and PSA for each station, which is critical for correctly averaging the data across stations to generate accurate GACC/PSA-level values and charts. This CSV is included in the repository download, and is pre-populated with default stations from the Northern California Geographic Area Coordination Center (ONCC). Users may freely edit this file to include any stations they wish to process.

While the tool was originally designed to support processing FEMS data at the GACC and PSA levels, these fields of the input CSV are flexible. Users simply need to ensure consistent naming for their chosen GACC and PSA values. For example, a PSA could represent a custom group of stations within a FDRA, Dispatch Boundary, or any other user-defined region. ***However, the original field names of the input CSV must not be changed***.

The below screenshot is an example of how the input CSV could be modified to download and process data for three FDRAs of the UTNUC (Northern Utah) dispatch boundary:

![screenshot_NFDRS_FEMS_1.png](/docs/screenshot_NFDRS_FEMS_1.png)



![screenshot_NFDRS_FEMS_2.png](/docs/screenshot_NFDRS_FEMS_2.png)

## Data Processing

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
   - Ex: from 03-01-2005 to 12-31-2022, there are 18 July 1st’s in the daily listing. The min/max/avg of these 18 values is calculated to determine the “Day Of Year” min/max/avg for each index.
   - Some refer to the 1-365 values as the “Julian Day” values, but “Day of Year” is more appropriate.
4. The daily listing is also used to determine the percentile breakpoints for each index
   - 1HrFM/10HrFM/100HrFM/1000HrFM: 40, 20, 10, 3, 1
   - All others: 60, 80, 90, 97, 100
   - Daily listing also used to create percentile lookup tables for each index
<br>

### PSA:
 - Process above is applied to only the group of stations within each PSA
 - Repeated for each PSA
<br>

### Station
 - Process above is applied to each individual station. However, since it is just an individual station, the historical record IS the “Daily Listing”. No averaging across stations is needed.
 - Repeated for each Station
<br>

## NFDRS Charts

Using the PercentileBreakpoint and DOY CSV files for each GACC/PSA/Station, charts are created for each of the user's specified indices.

The visual style of the charts was inspired by those made available via [Eric Drewitz's FireWxPy Python Library](https://pypi.org/project/firewxpy/)

![screenshot_NFDRS_FEMS_2.png](/docs/screenshot_NFDRS_FEMS_3.png)




## Usage

To use this toolbox:

1. [Download the repository](https://github.com/mpanunto/NFDRS_FEMS/archive/refs/heads/main.zip)
2. Extract NFDRS_FEMS.tbx
3. Run tool using ArcGIS Pro
