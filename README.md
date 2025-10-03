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

Users must provide the [StationList.csv](https://github.com/mpanunto/NFDRS_FEMS/blob/main/StationList.csv) file as input, which specifies the stations to include in the analysis. This CSV is included in the repository download, and is pre-populated with default stations from the **Northern California Geographic Area Coordination Center (ONCC)**. Users may freely edit this file to include any stations they wish to download and process. While the tool was originally designed to support processing FEMS data at the **GACC** and **PSA** levels, these fields of the input CSV are flexible. Users simply need to ensure consistent naming for their chosen **GACC** and **PSA** values. 

For example, a PSA could represent a custom group of stations within a FDRA, Dispatch Boundary, or any other user-defined region:

![screenshot_NFDRS_FEMS_1.png](/docs/screenshot_NFDRS_FEMS_1.png)

![screenshot_NFDRS_FEMS_2.png](/docs/screenshot_NFDRS_FEMS_2.png)


## Usage

To use this toolbox:

1. [Download the repository](https://github.com/mpanunto/NFDRS_FEMS/archive/refs/heads/main.zip)
2. Extract NFDRS_FEMS.tbx
3. Run tool using ArcGIS Pro
