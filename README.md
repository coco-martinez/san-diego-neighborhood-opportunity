# San Diego Development Opportunity Finder

## Overview

This project analyzes 727 census tracts across San Diego County to compare neighborhood conditions related to housing, transportation, walkability, schools, climate risk, demographics, and limited safety data.

The project started as an attempt to create one overall “development opportunity score,” but that became too broad. Instead, I built a first-pass screening tool using regression and clustering.

The regression model estimates expected rent based on tract conditions, then compares predicted rent with actual estimated rent. Tracts with much lower actual rent than predicted are flagged as places worth looking into more closely.

K-Means clustering is also used to group tracts into broader neighborhood types so similar areas are easier to compare.

## Data Sources

Main public data sources include:

- U.S. Census / ACS demographic and housing data
- City of San Diego police and transit data
- EPA walkability data
- FEMA National Risk Index data
- California public school, school district, and academic performance data

The final modeling dataset contains 727 tracts and 20 numeric features.

## Project Structure

```text
san-diego-development-opportunity-finder/
├── data/
│   ├── raw/              # original source files
│   └── processed/        # cleaned and modeling-ready outputs
├── notebooks/            # wrangling, EDA, preprocessing, and modeling
├── resources/            # report and presentation visuals
├── README.md
└── model_metrics.txt
```

## Modeling

I tested two regression models:

- Linear Regression
- Random Forest Regressor

The Random Forest was tuned using GridSearchCV, but Linear Regression performed slightly better on the test set and was easier to interpret.

Final Linear Regression results:

Test R²: 0.566
RMSE: about $364
MAE: about $278

I then used the final model to estimate rent across all 727 tracts. The bottom 10% of residuals, about -$415 or lower, flagged 73 tracts where actual rent was much lower than expected.


## Neighborhood Clusters

I also tested K-Means clustering and selected 3 clusters:

- Affluent Suburban Families
- Transit Family Renters
- Urban Renters

The silhouette scores were low, so these are broad neighborhood types rather than strict categories.

## How I'd Use It

This project is meant to narrow down a large set of tracts before doing more detailed research.

For example, I can filter for stronger safety scores, lower climate-loss risk, school quality, rent level, or neighborhood type, then map the remaining tracts to see where they are located and whether any geographic patterns stand out.

## Limitations

This is not a development success or property valuation model.

The analysis does not include parcel-level zoning, land cost, construction costs, permitting, unit condition, leasing data, or full countywide crime coverage. Safety data is only reliable within SDPD jurisdiction.

The regression model explains a little over half of the variation in rent, so the results should be treated as screening signals rather than exact predictions.

## Future Improvements

Future versions could include parcel and zoning data, permits, housing age and condition, coastal proximity, market trends, better school assignment data, and more detailed SANDAG GIS sources.

I'd also like to test different target variables and remove income from some models to better understand how the other neighborhood features relate to each other.
