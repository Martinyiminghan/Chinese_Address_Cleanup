# Luckin Coffee Location Data Cleansing for SAP

## Overview
This repository contains a data processing pipeline that cleans, standardizes, and geocodes across China. This is intended for SAP S4/HANA Upgrade mass processing of customer addresses. The script takes sample data from Kaggle for Luckin Coffee store locations and transforms it into a structured format suitable for SAP Customer Master Data (Business Partner, including Coordinates) integration. 

The core logic and workflow are demonstrated in `01_Location_Correction.md`.
The creation of address in Pinyin are demonstrated in `02_Generate_Pinyin_Output`.

## Features
- **Automated Data Retrieval**: Downloads the "5700 Luckin Coffee Stores Across China" dataset directly from Kaggle.
- **Address Standardization**: Integrates with the Amap (高德地图) API to fetch accurate, structured administrative divisions (Province, City, District) and street addresses.
- **Geocoding**: Extracts precise Longitude and Latitude coordinates for each store.
- **SAP-Ready Output**: Maps the cleaned data to standard SAP fields:
  - `BP code` (storeid)
  - `Province` (SAP Field REGION)
  - `City` (SAP Field CITY1)
  - `District` (SAP Field CITY2)
  - `Address` (SAP Field STREET)
  - `longitude` & `latitude` (SAP Field STREET)
- **Data Cleansing**: Automatically filters out invalid or unopened stores (e.g., entries marked as "敬请期待!").

## Prerequisites
- Python 3.8+
- An active [Amap (高德地图) Developer API Key](https://lbs.amap.com/)
- Kaggle account (for API access)

### Required Python Packages
```bash
pip install pandas requests kagglehub tqdm python-dotenv openpyxl
```

## Setup & Usage
1. Clone this repository to your local machine.
2. Create a `.env` file in the root directory and add your Amap API key:
   ```env
   AMAP_API_KEY= "your_amap_api_key_here"
   ```
3. Run the data processing script/notebook `01_Location_Correction.md`.
4. The script will output a standardized Excel file named `Processed Data.xlsx` in the root directory.

## Architecture & Workflow
1. **Data Ingestion**: Reads raw CSV data via `kagglehub`.
2. **API Enrichment**: Iterates through store records, intelligently querying the Amap API using the store name or raw address with `requests` and tracking progress with `tqdm`.
3. **Transformation**: Parses the JSON response, handles missing values, splits coordinates, and reshapes columns for SAP compatibility.
4. **Export**: Saves the final sanitized dataset to `.xlsx`.

## Contributing
Feel free to submit issues or pull requests if you have suggestions for improving the parsing logic or adding support for additional map APIs.
