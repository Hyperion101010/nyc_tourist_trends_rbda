# Collaborators

Developed in collaboration with [aygun najafova](https://github.com/aygunnajafova) as my team mate.

# NYC Tourist Trends
Data ingestion for the NYC tourism trends analysis starts with a MapReduce-driven cleanup of the raw sources (mta ridership, restaurant inspections, hotel reviews, and NYPD arrests) before the cleaned data lands in Hive tables for analytics.

## About the Dataset
This study uses four core datasets that span restaurant health inspections, hotel reviews, crime complaints, and subway ridership. Each dataset is publicly available, ranges from hundreds of megabytes to over a gigabyte, and contains millions of records with spatial attributes (ZIP, borough, coordinates) so we can align them geographically from MapReduce output into Hive.

### Dataset summary
- **NYC Restaurant Inspection**: ~150 MB with ~291k rows; includes CAMIS identifiers, restaurant metadata, inspection grades/scores, and precise spatial attributes from the DOHMH Open Data portal.
- **Hotel reviews (TripAdvisor via Kaggle)**: ~1 GB of scraped reviews with ~631k records, review text/ratings, hotel identifiers, and reviewer metadata that signal traveler sentiment.
- **NYPD Crime Complaints**: ~1.45 GB and 2+ million rows; contains complaint identifiers, timestamps, locations (borough/precinct/census tract), offense types, and descriptors for safety context.
- **MTA Subway Hourly Ridership**: >1 GB and ~121 million hourly records (2020–2024) with station IDs, ridership counts, timestamps, boroughs, and coordinates for mobility proxies.

### Background
We developed an integrated analytics pipeline that treats the four datasets as static snapshots, downloaded once from public portals or Kaggle, and stored locally for batch processing. The MapReduce stage normalizes fields such as ZIP codes, ridership, and inspection scores before writing into Hive staging tables, while the Hive scripts aggregate the cleansed data into neighborhood-level statistics, leisure/business scores, and the final dashboard export.

## Workflow
- `nyc_restaurent/`, `nypd_arrests/`: source folders for restaurant and crime data ingested via MapReduce.
- `group21_pct_A.hql`, `group21_nightlife.hql`, and `group21_final_master.hql`: Hive scripts that build restaurant, nightlife, and aggregate dashboards using the cleansed data.

## Workflow
- **MapReduce cleaning + ingestion**: the raw datasets in `mta_ridership/`, `nyc_restaurent/`, and `nypd_arrests/` are processed using classic MapReduce jobs to normalize key fields (zip codes, ratings, ridership) and load them into staging tables.
- **Hive analysis**: after the data lands in Hive, scripts such as `mta_ridership/group21_final_master.hql` combine and aggregate the cleansed tables to derive summary statistics, transit patterns, and the final dashboard dataset.

## Getting Started
1. Run the MapReduce pipelines that clean each source and write the results into Hive staging tables.
2. Execute `mta_ridership/group21_final_master.hql` (or the other grouped Hive scripts) to build the derived tables, dashboard dataset, and exports.

## Directory pointers
- `mta_ridership/`: Hive queries for ridership-driven joins and exports.
- `nyc_restaurent/`, `nypd_arrests/`: source folders for restaurant and crime data ingested via MapReduce.

