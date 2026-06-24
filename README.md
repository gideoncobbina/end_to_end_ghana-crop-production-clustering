# ghana-crop-production-clustering

## Overview

This project looks at crop production data for Ghana from 2013 to 2017. It cleans the raw data, explores it with simple charts, and then groups districts into farming types using clustering. The result is a small set of district groups, such as Root and tuber belt or Northern cereal-legume, that describe how each district actually farms, based on the data rather than on assumptions.

The project is built to be readable by a beginner and useful to a recruiter at the same time. The code favors clear function names and short steps over clever shortcuts. The Power BI dashboard that follows this analysis is deliberately small: four pages, a few charts each, built to be understood in under two minutes.

## Why This Project Matters

Ghana's agriculture sector is not uniform. A district in the north growing mostly cereals and legumes faces different risks and opportunities than a district in the forest belt growing mostly root crops. Treating all districts the same in a report hides that difference. Grouping districts by their actual crop mix gives a more honest, more useful picture for anyone planning support programmes, comparing regions, or simply trying to understand how farming varies across the country.

## What the Notebook Does

The notebook is organized into five steps.

### Step 0: Import the tools
Loads pandas, numpy, matplotlib, and the scikit-learn tools needed for clustering.

### Step 1: Load the data and look at it
Reads the raw CSV file and checks its shape. The raw file has known problems: duplicate rows, numbers stored as text with extra spaces, and missing values written as a dash instead of being left blank.

### Step 2: Clean the data
Five clear actions, done in order:
- Column names are shortened and made consistent, for example AREA (Ha) becomes area_ha
- Text columns are trimmed of extra spaces and given consistent capitalization
- Numbers stored as text are converted to real numbers, and dashes are treated as missing values
- Duplicate rows are removed
- A small number of production values are rebuilt, because production should always equal area multiplied by yield, and some original values did not match that rule. Rows that were corrected are flagged in a new column, so nothing is changed silently.

### Step 3: Explore the data with charts
Before any clustering happens, simple charts answer the basic questions: which regions produce the most, how production changed year by year, which crops produce the most overall, and which crops give the best yield per hectare. Crops are also grouped into three families (cereal, root, legume) to make later comparisons easier.

### Step 4: Group districts into farming types
This is the clustering step. Instead of grouping individual rows, the notebook builds one summary profile per district: what share of its land grows cereals, what share grows root crops, what share grows legumes, its average yield, its total farmed area, and how many different crops it grows. These profiles are scaled and passed into a K-Means model. The number of groups (four) was chosen by checking the silhouette score, which measures how cleanly the groups separate.

Each group is then given a plain-language name based on its actual profile, not an arbitrary number:
- Root and tuber belt: at least 60 percent of land in root crops
- Northern cereal-legume: a strong mix of cereal and legume land
- Mixed transitional: a mix of cereal and root crop land, in between the other two patterns
- Urban / peri-urban: small total farmed area with a high cereal share
- Other: districts that do not clearly fit any of the above patterns

A scatter chart and a region-by-group table are used to check that the grouping makes sense, both visually and geographically.

### Step 5: Save the results
Two files are saved:
- crop_clean.csv: every original row, cleaned, with each district's group name attached
- district_clusters.csv: one row per district, with its profile numbers and group name

## Honest Limitations

The notebook ends with a short list of things to keep in mind when using these results:

- The data only covers 2013 to 2017, and 2016 has noticeably less data than the other years
- The regions used are Ghana's original 10 regions, before the country was reorganized into 16 regions in 2018
- A small number of production values were rebuilt because the originals clearly did not match area times yield
- The district groups are based on all five years combined, so they describe the general type of farming in a district, not how that farming changed year to year

## Project Files

```
.
├── ghana_crop_analysis_beginner.ipynb      Full notebook: clean, explore, cluster, save
├── crop_clean.csv                          Cleaned data, every row, with group name attached
├── district_clusters.csv                   One row per district, with profile and group name
└── README.md
```

## Tools Used

- Python 3
- pandas and numpy for data cleaning and reshaping
- matplotlib for charts inside the notebook
- scikit-learn (StandardScaler, KMeans, silhouette_score) for clustering
- Power BI Desktop for the dashboard

## How to Run This Project

1. Clone the repository: `git clone https://github.com/gideoncobbina/ghana-crop-production-clustering.git`
2. Open ghana_crop_analysis_beginner.ipynb in Jupyter Notebook or Google Colab
3. Set RAW_PATH near the top of the notebook to the location of your CSV file
4. Run the notebook from top to bottom
5. Confirm that crop_clean.csv and district_clusters.csv are created
6. Open Power BI Desktop and follow Ghana_CropProduction_PowerBI_Guide.pdf to build the dashboard

## About the Power BI Guide

The guide gives the exact position and size of every visual on a 12 by 8 layout grid, so the dashboard can be rebuilt the same way every time. It also explains the business reasoning behind each chart: not just how to build it, but why a recruiter or analyst would expect to see it. The dashboard is kept to four pages on purpose, since a small set of clear insights demonstrates analytical judgment better than a large dashboard packed with charts.
