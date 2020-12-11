#### Capstone Final Project 
#### Udacity 2020 

#### Project Summary

This project is desinged to manipulate data from US immigration to discover popular cities for immigration. Second, demographic information to analyze the gender of the immigrants. Along with the visa types and basic details of age. Lastly, we extracted data for the average temperature per city. 

#### 3 Dataset breakdown:

1) i94 dataset of 2016 
2) Kaggle - average temperature per city
3) OpenSoft - city demographic data 

* 3 different sources of data extraction are required

- Star Schema will be used. 

#### Data sources

The main dataset will include data on immigration to the United States, and supplementary datasets will include data on airport codes, U.S. city demographics, and temperature data. 

1. **I94 Immigration Data**: The dataset contains data from 2016 from the U.S. National Tourism and Trade Office. [link](https://travel.trade.gov/research/reports/i94/historical/2016.html)

2. **World Temperature Data**: Comes from Kaggle and contains average weather temperatures by city. [link](https://www.kaggle.com/berkeleyearth/climate-change-earth-surface-temperature-data)

3. **U.S. City Demographic Data**: comes from OpenSoft and contains information about the demographics of all US cities such as average age, male and female population. [link](https://public.opendatasoft.com/explore/dataset/us-cities-demographics/export/)

####  Data Model
##### Staging Tables

staging_i94_df
    - id
    - date
    - city_code
    - state_code
    - age
    - gender
    - visa_type
    - count
    
staging_temp_df
    - year
    - month
    - city_code
    - city_name
    - avg_temperature
    - lat
    - long
    
staging_demo_df
    - city_code
    - state_code
    - city_name
    - median_age
    - pct_male_pop
    - pct_female_pop
    - pct_veterans
    - pct_foreign_born
    - pct_native_american
    - pct_asian
    - pct_black
    - pct_hispanic_or_latino
    - pct_white
    - total_pop


##### Dimension Tables

immigrant_df
    - id
    - gender
    - age
    - visa_type
    
city_df
    - city_code
    - state_code
    - city_name
    - median_age
    - pct_male_pop
    - pct_female_pop
    - pct_veterans
    - pct_foreign_born
    - pct_native_american
    - pct_asian
    - pct_black
    - pct_hispanic_or_latino
    - pct_white
    - total_pop
    - lat
    - long
    
monthly_city_temp_df
    - city_code
    - year
    - month
    - avg_temperature
    
time_df
    - date
    - dayofweek
    - weekofyear
    - month


##### Fact Table

immigration_df
    - id
    - state_code
    - city_code
    - date
    - count


#### Steps necessary to pipeline the data into the chosen data model

1. Clean the data on nulls, data types, duplicates, etc
2. Load staging tables for staging_i94_df, staging_temp_df and staging_demo_df
3. Create dimension tables for immigrant_df, city_df, monthly_city_temp_df and time_df
4. Create fact table immigration_df with information on immigration count, mapping id in immigrant_df, city_code in city_df and monthly_city_temp_df and date in time_df ensuring referential integrity
5. Save processed dimension and fact tables in parquet for downstream query

#### Discussions

Q1: Clearly state the rationale for the choice of tools and technologies for the project.

A: Spark quickly performs processing tasks on large databases. It can also perform these tasks on different data formats - CSV, SAS and Parquet (required in this project 2 different types). Spark SQL was used to manipulate the files into dataframes and create tables with join operations. 

-------------------

Q2: Propose how often the data should be updated and why.

A: This particular type of data set some data could be pulled only when need and the some could be pulled on a monthly basis. For example the monthly city temperature can be pulled on a monthly basis as new data will be available. Where as the immigration dataset would be done later as it requires more time to collect that data. 


-------------------

#### Write a description of how you would approach the problem differently under the following scenarios:

Q3: The data was increased by 100x.

A:  With an increase in data size storage space and accessibility to the data is important. Using Amazon Redshift or EC2 hosting Spark will give enough storage to hold the data sets. 


-------------------

Q4: The data populates a dashboard that must be updated on a daily basis by 7am every day.

A:  Airflow can be automated to schedule regular data tasks(e.g at 7am daily). Setting up the DAG can be done to accomplish a variety of tasks and notifications on particular data set through emails, messages etc. 


-------------------

Q5: The database needed to be accessed by 100+ people.

A: Data warehouse in the cloud could be a possible solution as it gives the data access to more users with larger server capacity and good read performance. Also allowing people who are unable to store the data locally can gain access to the data cloud based.  