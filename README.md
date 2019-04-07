# Data Modeling with PostgreSQL

### Introduction

In order to answer the following questions:

1. What songs are users listening to?
2. What activites user spend the most time on?

### Project Objective

The data resides in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in the app.

In order to answer the questions mentioned above, I created a database schema and ETL pipeline for the analysis.

1. Create a Postgres database with defined fact and dimension tables applying Star Schema
2. Build an ETL pipeline that transfers data into these tables in Postgres using Python and SQL



### Why SQL?

- Easier to change to business requirements- perform `adhoc queries` with ease
- Modeling the data not modeling queries
- ACID transactions: Guarantee validity even in event of errors, power failures



### Explore the datasets

##### 1. Song Dataset

The first dataset is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID. For example, here are filepaths to two files in this dataset.

```txt
song_data/A/B/C/TRABCEI128F424C983.json
song_data/A/A/B/TRAABJL12903CDCF1A.json
```

And below is an example of what a single song file, TRAABJL12903CDCF1A.json, looks like.

```json
{"num_songs": 1, 
 "artist_id": "ARJIE2Y1187B994AB7", 
 "artist_latitude": null, 
 "artist_longitude": null, 
 "artist_location": "", 
 "artist_name": "Line Renaud", 
 "song_id": "SOUPIRU12A6D4FA1E1", 
 "title": "Der Kleine Dompfaff", 
 "duration": 152.92036, 
 "year": 0}
```

##### 2. Log Dataset

The second dataset consists of log files in JSON format. These describe app activity logs from a music streaming app based on specified configurations.

The log files in the dataset are partitioned by year and month. 

For example, here are filepaths to two files in this dataset.

```txt
log_data/2018/11/2018-11-12-events.json
log_data/2018/11/2018-11-13-events.json
```

And below is an example of what the data in a log file, 2018-11-12-events.json, looks like.

![](https://ws4.sinaimg.cn/large/006tNc79ly1g1tnjyob18j316b0cltcm.jpg)



### Schema for Song Played Analysis

Using the song and log datasets, I created a star schema optimized for queries on song play analysis. This includes the following tables.

##### 1. Fact Table

1. songplays

   - records in log data associated with song plays i.e. records with page `NextSong`

   - *songplay_id*
   - *start_time*
   - *user_id*
   - *level*
   - *song_id*
   - *artist_id*
   - *session_id*
   - *location*
   - *user_agent*

##### 2. Dimension Tables

1. users - users in the app
   - *user_id*
   - *first_name*
   - *last_name*
   - *gender*
   - *level*
2. songs - songs in music database
   - *song_id*
   - *title*
   - *artist_id*
   - *year*
   - *duration*
3. artists - artists in music database
   - *artist_id*
   - *name*
   - *location*
   - *lattitude*
   - *longitude*
4. time - timestamps of records in songplays broken down into specific units
   - *start_time*
   - *hour*
   - *day*
   - *week*
   - *month*
   - *year*
   - *weekday*

---

# How to use this project

How to install and set up Postgres locally in case you want to follow along the project on your local machine. This [link](https://www.codementor.io/engineerapart/getting-started-with-postgresql-on-mac-osx-are8jcopb) provides it for MacOs. It goes through configuring Postgres, creating users, creating databases using the psql utility.

Perform ETL development in `etl.ipynb` and `test.ipynb`

1. Run `create_tables.py` to create database and tables 

   (Alter queries if you want in `sql_queries.py`)

2. Run `etl.py` to perform the complete ETL pipeline
