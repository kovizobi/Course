Taxi Trip Data Aggregation Project

Overview
This project processes and aggregates data from multiple sources related to taxi trips in Chicago. It integrates data from various files (taxi trips, weather, company, payment type, and community areas) 
and performs several transformations. The result is a comprehensive dataset that includes information about taxi trips, weather conditions, community area data, and other related attributes.

Features
Company Master Update: Automatically updates the company_master DataFrame by adding new companies from the taxi trips dataset.
Payment Type Master Update: Automatically updates the payment_type_master DataFrame by adding new payment types from the taxi trips dataset.
Weather Data Transformation: Converts raw weather API data into a structured DataFrame for easy analysis.
Data Merging: Combines various data sources (taxi trips, weather data, company information, community areas, and payment types) into a single, aggregated dataset.
Output: The final aggregated dataset is saved as a CSV file for easy access.

Installation
Prerequisites
Before running the project, you need to install the following Python libraries:
pandas – For data manipulation and analysis.
boto3 – AWS SDK for interacting with S3 buckets.
requests – For making HTTP requests to the weather API.
python-dotenv – To manage environment variables for sensitive credentials.

To install the required libraries, use pip:
pip install pandas boto3 requests python-dotenv

Setting up AWS Credentials
You will need AWS credentials to interact with S3. Store your AWS access and secret keys in a .env file in the root of the project. The .env file should look like this:
AWS_ACCESS_KEY=your_aws_access_key
AWS_SECRET_KEY=your_aws_secret_key
You can obtain AWS credentials from the AWS Management Console.

Usage
Running the Project
Prepare Data Sources: Ensure the data files are available in the AWS S3 bucket. The following data is required:

Data Sources
Taxi Trips - data related to individual taxi trips, including information like trip duration, pickup/dropoff locations, payment method, and company.
Weather Data - hourly weather data (temperature, wind speed, rain, etc.) for Chicago, sourced from the Open-Meteo API.
Community Areas - data about Chicago's community areas, including community names and area codes.
Company Master - contains a list of companies providing taxi services in the region.
Payment Type Master - contains various payment methods available in the taxi service.
Date - date dimension table, a specialized table to store detailed information about dates.

Script Execution: Once the data is available in the S3 bucket, you can execute the 08_local_visualisations.ipynb script to update the master tables, process the taxi trips, and create the aggregated 
trips_full dataset.

Steps:
- Import necessary libraries: os, StringIO, boto3, pandas, and dotenv.
- Loaded environment variables using load_dotenv() to access AWS keys)
- Retrieve AWS credentials using os.getenv
- Create an AWS S3 client with boto3
- Define a utility function read_csv_from_s3(bucket, path, filename)
  to downloads and reads CSV files from AWS S3 into Pandas DataFrame.
- Load the following CSVs from specified S3 paths:
	community_areas_master.csv
	company_master.csv
	weather_data_date.csv
	payment_type_master.csv
- Load and concatenate taxi trips data iterating through CSV files 
  in the taxi_trips folder on S3.
- Load and concatenate taxi weather data iterating through CSV files 
  in the weather folder on S3.
- Merge all relevant data, taxi trips and weather, taxi trips and and
   company info, taxi trips and and payment type, taxi trips and and 
   pickup community area names,  taxi trips and and dropoff community area names
- Prepare and merged date info, converted datetime strings to actual datetime 
  objects in both date and taxi trips
- Save final enriched taxi trips to a local CSV file: trips_full.csv

All other scripts perform auxiliary functions using local storage separately from the S3 bucket. The function of each script is clarified in its heading.

S3 bucket Structure
The default access path in S3 bucket is set to transformed_data/(data access path)/.
The data access path is set to the following values by default:
taxi_trips/ - taxi trips data
weather/ - weather data (from Open-Meteo API)
company/ - company master data
payment_type/ - payment type master data
community_areas/ - community areas data
date/ - date dimension table 

File lists
01_json_scraping.ipynb - performs JSON data extraction by reading and parsing a local spotify_playlist.json file
02_web_scraping.ipynb - performs web scraping to extract a table of Chicago community areas from a Wikipedia page and saves it as a CSV file
03_get_taxi_data_v2.ipynb - retrieves taxi trip data from the City of Chicago’s open data API
04_get_weather_data.ipynb - retrieving and processing historical weather data using the Open-Meteo ERA5 reanalysis API.
05_date_dimension.ipynb - creating a Date Dimension Table, a fundamental component in time-based data analysis.
06_trip_data_mapping.ipynb - performs data loading, cleaning, transformation for taxi trip data from the City of Chicago's open data portal. 
07_transform_load.ipynb - a complete ETL (Extract, Transform, Load) pipeline for processing Chicago taxi trip and weather data.
08_local_visualisations.ipynb -  compiles an aggregated table of taxi trips in Chicago by pulling in and joining several datasets from AWS S3.
09_five visualizations.ipynb - creates five visualizations using the enriched taxi trip dataset (trips_full.csv) to explore patterns and insights in Chicago taxi data.

License
This project is not currently licensed under any open-source license.
