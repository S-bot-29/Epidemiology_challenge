# README Epidemiology_challenge
Repository for Data Mastery challenge on covid epidemiology indicators and wastewater monitoring
This README file explains how to run the analysis, the expected outputs, and troubleshooting steps for the provided R scripts. 
________________________________________
Overview
This project contains a set of R scripts used for processing, analyzing, and visualizing COVID-19-related data at different levels (municipality, security region, and national). The pipeline integrates multiple datasets, including population statistics, case counts, wastewater RNA levels, and ICU admissions as well as a municipality shapefile and some files that necessary to help joining the different datasets.
The scripts included:
1.	Create_population_table.R – Cleans and preprocesses the population dataset.
2.	Municipality_level.R – Processes and merges case counts, population, and wastewater data at the municipality level.
3.	Security_region_level.R – Aggregates and processes the data at the security region level.
4.	National_level.R – Processes data at the national level, normalizing cases per 100,000 inhabitants.
5.	National_level_visualization.R – Generates visualizations of key epidemiological indicators.
________________________________________
1. How to Run the Analysis
Step 1: Set Up the Environment
Ensure that you have R installed along with the necessary packages:
install.packages(c("ggplot2", "dplyr", "readxl", "lubridate", "fuzzyjoin", "spatialEco", "shiny", "scales", "hrbrthemes", "anytime", "stringr"))
Make sure you have set the working directory properly in each script:
setwd("your/directory/path")
Replace "your/directory/path" with the location where your input datasets are stored.

Step 2: Prepare Population Data
Run Create_population_table.R to clean and structure the population dataset. This script:
•	Reads an Excel file containing population statistics.
•	Cleans and renames the columns.
•	Saves the cleaned dataset as population.csv.
source("Create_population_table.R")

Step 3: Process Data at Different Levels
Run the following scripts sequentially:
Municipality Level
source("Municipality_level.R")
•	Reads COVID-19 case counts, wastewater data, and population data.
•	Merges datasets and normalizes case counts by population.
•	Saves the processed dataset as Final_data_municipality.csv.
Security Region Level
source("Security_region_level.R")
•	Aggregates case counts at the security region level.
•	Merges with wastewater data.
•	Outputs Final_data_region.csv.
National Level
source("National_level.R")
•	Aggregates data at the national level.
•	Normalizes case counts per 100,000 inhabitants.
•	Saves Final_data_national.csv.

Step 4: Generate Visualizations
To generate visualizations, run:
source("National_level_visualization.R")
This script:
•	Reads Final_data_national.csv.
•	Creates plots comparing reported cases, hospital admissions, and deaths to wastewater RNA levels.
________________________________________
2. Expected Outputs
The analysis produces the following outputs:
Output File	Description
population.csv	Cleaned population data with standardized municipality codes.
Final_data_municipality.csv	Municipality-level COVID-19 case counts merged with population and wastewater data.
Final_data_region.csv	Aggregated security region-level dataset.
Final_data_national.csv	National-level dataset with normalized case counts and wastewater data.
Graphs (from National_level_visualization.R)	Time series plots of case counts, hospital admissions, and wastewater RNA trends.
________________________________________

3. Troubleshooting
Common Issues and Fixes
Issue	Possible Cause	Solution
Error in read_excel: File not found	The file path is incorrect.	Update file_path in Create_population_table.R to the correct location.
Error: object not found	Missing datasets.	Ensure all input files are correctly placed in the working directory.
join() error: Column types do not match	Data types mismatch in merging operations.	Convert columns to character format before merging (as.character()).
Error: cannot open file	File is open in another application.	Close the file and re-run the script.
ggplot2 Error: Object not found	The data frame column name may be incorrect.	Check if column names match expected headers using colnames(dataset).
________________________________________

4. Notes
•	Ensure that input datasets are correctly formatted (.csv files use ";" as a separator).
•	When running scripts on different systems, update file paths accordingly.
•	Date columns should always be formatted properly using: 
•	as.Date(date_column, format="%Y-%M-%D")
•	If an error occurs while merging, check for missing values using: 
•	sum(is.na(dataset$column_name))
________________________________________
Due to the unreliability of our dataset, a direct comparison of results with Hassan (2021) for validation purposes or pattern analysis between epidemiological factors and COVID-19 RNA levels in wastewater is not meaningful. However, a cursory visual comparison highlights discrepancies in our dataset, with our epidemiological values appearing unreasonably or even impossibly high. These errors suggest fundamental data processing issues that impacted our results.
Our dataset's unreliability stems from the fact that many of our normalized epidemiological values significantly exceed reasonable limits, sometimes more than three times the normalization benchmark of 100,000 inhabitants. Upon reviewing our data tables, we found that these anomalies were already present before normalization, with epidemiological indicators in some cases exceeding the total population size.
Because there seemed to be no csv file for municipality populations in 2022 available, we had to create and adapt an existing .xlx file into a csv file and filter out the noise in that file. We managed to accomplish this. Some inconsistencies required data transformation/ calibration at pre-analysis stage. Even if we had used the correct dataset, we still would have had to make changes in the code in order to incorporate hospital admissions in the national dataset. Originally handling data structure difference seemed to have been done successfully, until we discovered we used the wrong dataset. Because we interpreted the data in the COVID-19_aantallen_gemeenten_cummulatief wrongly, our final dataset turned out to be unreliable. We weren't able to fix this in time, but we do know how to fix it.

