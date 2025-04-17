# Balancing Public Health Needs and Private Concerns  
**Wendy Shi, Giulia Pullano, Muhammad Saad**

This repository contains all Python code written by Wendy Shi for the Bansal Lab project *"Balancing Public Health Needs and Private Concerns."* The following sections introduce the folder structure and describe the contents of each directory.

## `State` Folder  
Currently, analysis has been completed for three states. Each state folder contains a set of Python files (with names varying by state) that follow the same workflow and functionality.  
The code files are named in the order of execution. For each state, we focus on generating a weight matrix and visualizations for counties with populations larger than 10,000.

### Example: `01New_York/`

### Python Code Files:
- **01_skmob_NY.ipynb**  
  Generates synthetic trajectories for 10,000 individuals in New York, using population density data at the Census Block Group (CBG) level.  
  > *Note: This notebook should be run in a virtual Conda environment with the `scikit-mobility` package installed. See [scikit-mobility GitHub](https://github.com/scikit-mobility/scikit-mobility) for setup instructions. Due to file size, synthetic trajectory data is not included in this repository.*

- **02_ipykernel_NY_GIS.ipynb**  
  Aggregates visit data at the CBG level. Main steps:
  - Map latitude/longitude coordinates to CBGs using 12-digit GEOIDs.
  - Assign origin (5-digit FIPS of home CBG) and destination (first 5 digits of visited GEOID).
  - Group by day and user ID to count visits per CBG.
  - Add Laplace noise to CBG-level visit counts for different ε values (ranging from 0.8 to 0.005).
  - Aggregate noised visits to the county level (55 counties in total).
  - For each day, generate a 55×55 Origin-Destination (OD) matrix showing total visits from county *i* to *j*.
  - Average the OD matrix across all 30 days for each ε level.

- **03_Noise_Aggregate_Data.ipynb**  
  Adds noise to the aggregated weight matrix for the *n* counties in each state.

- **04_Matrix_Transformation_Visualization_individual.ipynb**  
  Visualizes `p_ii`, `p_ij`, and the proportion of zero links for each ε value, using data with noise added at the individual level.

- **05_Matrix_Transformation_Visualization_Aggregated.ipynb**  
  Same as above, but using data with noise added at the aggregated level.

- **06_Data_Collection_Simulations.ipynb**  
  Implements an SEIR metapopulation model, storing S, E, I, and R values for all *n* counties over 50 days starting from May 15.

- **07_Arrival_Time_Visualization.ipynb**  
  Visualizes the distribution of arrival and peak infection dates for the *n* counties.

---

### Data

- **ACSDT5Y2020/**  
  Population density data at the CBG level, downloaded from [data.census.gov](https://data.census.gov/).  
  Steps:
  - Search for "population"
  - Under “Geography,” select “Block Group”
  - Choose the relevant state
  - Select desired counties and block groups
  - Download the latest 5-Year ACS Estimates (table B01003 "Total Population" is recommended)

- **tl_2020_36_bg/**  
  Shapefile for Census Block Groups, also from [census.gov](https://www.census.gov).  
  - Go to *Data & Maps > Mapping Files*
  - Under 2020, select *TIGER/Line Shapefiles*
  - Use the web interface to select "Census Block Group" and your state  
  > *Note: "36" is the FIPS code for New York. This will vary across states. Due to file size, the New York shapefiles is not included in this repository.*

---

### Output

- **State_Data/**
  - **Aggregate/**: Contains the weight matrices and SEIR simulation results for each ε level, using aggregated data with noise.
  - **Individual/**: Same as above, but with noise added at the individual data level.

- **Visualization/**
  - **Aggregate/**: Includes visualizations of `p_ii`, `p_ij`, zero-link proportions, and box plots of arrival/peak times based on aggregated data.
  - **Individual/**: Same as above, but using data with individual-level noise.

- **State_SEIR_name.csv**  
  Contains population sizes and initial SEIR conditions for each county in a given state.

---

## `Data` Folder

- **seed_1503_seed_corrected_undereporting_CDC_corrected_v3.txt**  
  Initial SEIR model conditions (E values) for all states.

- **nodes_considered.csv**  
  Maps 5-digit FIPS codes to data indices and stores the monthly average values used for comparison.  
  > *Note: Comparison data is not included due to file size.*

- **us_shapefile_county.csv**  
  Lookup table with state names, populations, and FIPS codes for all U.S. counties.

> *Note: To compute the real aggregated mobility matrix using SafeGraph data, please download the 12 monthly averaged matrices `monthly_matrix_sd_county_month_county.txt`.
For access to the file, please contact Giulia or Wendy directly.*
