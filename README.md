# template-activity-03


# Query Project

- In the Query Project, you will get practice with SQL while learning about
  Google Cloud Platform (GCP) and BiqQuery. You'll answer business-driven
  questions using public datasets housed in GCP. To give you experience with
  different ways to use those datasets, you will use the web UI (BiqQuery) and
  the command-line tools, and work with them in jupyter notebooks.

- We will be using the Bay Area Bike Share Trips Data
  (https://cloud.google.com/bigquery/public-data/bay-bike-share). 

#### Problem Statement
- You're a data scientist at Ford GoBike (https://www.fordgobike.com/), the
  company running Bay Area Bikeshare. You are trying to increase ridership, and
  you want to offer deals through the mobile app to do so. What deals do you
  offer though? Currently, your company has three options: a flat price for a
  single one-way trip, a day pass that allows unlimited 30-minute rides for 24
  hours and an annual membership. 

- Through this project, you will answer these questions: 
  * What are the 5 most popular trips that you would call "commuter trips"?
  * What are your recommendations for offers (justify based on your findings)?


## Assignment 03 - Querying data from the BigQuery CLI - set up 

### What is Google Cloud SDK?
  
  Very good reading!
  
- Read: https://cloud.google.com/sdk/docs/overview

- If you want to go further, https://cloud.google.com/sdk/docs/concepts has
  lots of good stuff.

### Get Going

- Install Google Cloud SDK: https://cloud.google.com/sdk/docs/

- Try BQ from the command line:

  * General query structure

    ```sql
    bq query --use_legacy_sql=false '
        SELECT count(*)
        FROM
           `bigquery-public-data.san_francisco.bikeshare_trips`'
    ```

### Queries

1. Rerun last week's queries using bq command line tool (Paste your bq
   queries):

- What's the size of this dataset? (i.e., how many trips)
```sql
  bq query --use_legacy_sql=false "SELECT count(*) FROM `bigquery-public-data.san_francisco.bikeshare_trips`"
```

- What is the earliest start time and latest end time for a trip?
```
  bq query --use_legacy_sql=false "SELECT min(start_date) FROM `bigquery-public-data.san_francisco.bikeshare_trips`"
  bq query --use_legacy_sql=false "SELECT max(end_date) FROM `bigquery-public-data.san_francisco.bikeshare_trips`"
```

- How many bikes are there?
```
  bq query --use_legacy_sql=false "SELECT count(distinct bike_number) FROM `bigquery-public-data.san_francisco.bikeshare_trips`"
```

2. New Query (Paste your SQL query and answer the question in a sentence):

- How many trips are in the morning vs in the afternoon?
```
  bq query --use_legacy_sql=false "SELECT count(trip_id) as AM_Count FROM `bigquery-public-data.san_francisco.bikeshare_trips` WHERE EXTRACT(HOUR FROM start_date)^<12"
```
```
  bq query --use_legacy_sql=false "SELECT count(trip_id) as PM_Count FROM `bigquery-public-data.san_francisco.bikeshare_trips` WHERE EXTRACT(HOUR FROM start_date)^>=12"
```


### Project Questions
Identify the main questions you'll need to answer to make recommendations (list
below, add as many questions as you need).

- Question 1: 
 * What are the most popular zipcodes?

- Question 2: 
 * What are the popular zipcodes by hours?

- Question 3: 
 * Which zipcodes have the most total distance?

- Question 4: 
 * What are the most popular landmarks?

- Question 5: 
 * How many stations by landmark?


### Answers

Answer at least 4 of the questions you identified above You can use either
BigQuery or the bq command line tool.  Paste your questions, queries and
answers below.

- Question 1: what are the top 3 zipcodes which have the most trips?
  * Answer: 94107, 94105, 94133
  * SQL query:
  ```sql
    SELECT count(zip_code) as count, zip_code FROM [bigquery-public-data:san_francisco.bikeshare_trips]
    group by zip_code
    order by count desc
  ```

- Question 2: What are the popular zipcode by hours?
  * Answer: The top hours by zipcodes is 94107 8am, 9am, 5pm, 6pm
  * SQL query:
  ```sql
    SELECT hour(start_date) as hour, count(hour(start_date)) as count, zip_code FROM [bigquery-public-   data:san_francisco.bikeshare_trips]
    Group by hour, zip_code, hour
    Order by count DESC
  ```

- Question 3: Which zipcode have the most total distance?
  * Answer: 95531
  * SQL query:
  ```sql
    SELECT zip_code, duration_sec FROM [bigquery-public-data:san_francisco.bikeshare_trips]
    Group by zip_code, duration_sec
    Order by duration_sec DESC
  ```

- Question 4: How many stations by landmark?
  * Answer: San Jose 18, Palo Alto 5, Mountain View 7, Redwood City	7, San Francisco 37
  * SQL query:
  ```
    SELECT landmark, count(station_id) as Station_Count FROM [bigquery-public-data:san_francisco.bikeshare_stations]
    Group by landmark
  ```

