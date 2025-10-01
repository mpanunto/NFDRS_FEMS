***Latest version is v20250930***

# NFDRS_FEMS

Tool that automates the download and processing of FEMS data for user-specified stations. For each selected fire danger index, it generates:

- Daily listings  
- Percentile breakpoint tables  
- Percentile lookup tables  
- NFDRS charts

Calculations are performed at three geographic scales:

- **GACC** (Geographic Area Coordination Center)  
- **PSA** (Predictive Service Area)  
- **Station**

## Input Requirements

Users must provide a [StationList.csv](https://github.com/mpanunto/NFDRS_FEMS/blob/main/StationList.csv) file as input, which specifies the stations to include in the analysis.

A sample CSV is included, pre-populated with default stations from the **Northern California Geographic Area Coordination Center (ONCC)**. Users can freely edit this file to include any stations they wish to download and process.

While the tool is designed to support processing at the **GACC** and **PSA** levels, these fields are flexible — users simply need to ensure consistent naming for their chosen **GACC** and **PSA** values. For example, a PSA could represent a custom grouping of stations within a Dispatch Boundary, FDRA, or any other user-defined region.




## Usage

To use this toolbox:

1. [Download the repository](https://github.com/mpanunto/NFDRS_FEMS/archive/refs/heads/main.zip)
2. Extract NFDRS_FEMS.tbx
3. Run tool using ArcGIS Pro
