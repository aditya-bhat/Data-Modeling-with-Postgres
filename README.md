# Data-Modeling-with-Postgres

# Problem Statement:

A startup called Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. The analytics team is particularly interested in understanding what songs users are listening to. Currently, they don't have an easy way to query their data, which resides in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

They'd like a data engineer to create a Postgres database with tables designed to optimize queries on song play analysis, and bring you on the project. Your role is to create a database schema and ETL pipeline for this analysis. You'll be able to test your database and ETL pipeline by running queries given to you by the analytics team from Sparkify and compare your results with their expected results.

### Data

- **Song datasets**: all json files are nested in subdirectories under _/data/song_data_. A sample of this files is:

```
{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}
```

- **Log datasets**: all json files are nested in subdirectories under _/data/log_data_. A sample of a single row of each files is:

```
{"artist":"Slipknot","auth":"Logged In","firstName":"Aiden","gender":"M","itemInSession":0,"lastName":"Ramirez","length":192.57424,"level":"paid","location":"New York-Newark-Jersey City, NY-NJ-PA","method":"PUT","page":"NextSong","registration":1540283578796.0,"sessionId":19,"song":"Opium Of The People (Album Version)","status":200,"ts":1541639510796,"userAgent":"\"Mozilla\/5.0 (Windows NT 6.1) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/36.0.1985.143 Safari\/537.36\"","userId":"20"}
```

## Database Schema

The schema used for this exercise is the Star Schema:
There is one main fact table containing all the measures associated with each event _songplays_,
and 4-dimensional tables _songs_, _artists_, _users_ and _time_, each with a primary key that is being referenced from the fact table.

On why to use a relational database for this case:

- The data types are structured (we know before-hand the structure of the jsons we need to analyze, and where and how to extract and transform each field)
- The amount of data we need to analyze is not big enough to require big data related solutions.
- This structure will enable the analysts to aggregate the data efficiently
- Ability to use SQL that is more than enough for this kind of analysis
- We need to use JOINS for this scenario

### Requirements for running locally

- Python3
- Docker
- Docker-Compose

### Project structure explanation

```
Data-Modeling-with-Postgres
│   README.md                # Project description
│   docker-compose.yml       # Postgres container description
│
└───data                     # The dataset
|   |
│   └───log_data
│   |   │  ...
|   └───song_data
│       │  ...
│
└───src                     # Source code
|   |
│   └───notebooks           # Jupyter notebooks
│   |   │  etl.ipynb        # ETL helper notebook
|   |   |  test.ipynb       # Psql queries notebook
|   |   |
|   └───scripts
│       │  create_tables.py # Schema creation script
|       |  etl.py           # ETL script
|       |  sql_queries.py   # Definition of all sql queries
```

### Instructions for running locally

Clone repository to local machine

```
git clone https://github.com/aditya-bhat/Data-Modeling-with-Postgres.git
```

Change directory to local repository

```
cd Data-Modeling-with-Postgres
```

Create python virtual environment

```
python3 -m venv venv             # create virtualenv
source venv/bin/activate         # activate virtualenv
pip install -r requirements.txt  # install requirements
```

Start postgres container

```
docker-compose up  # run this command in new terminal window or tab
```

Run scripts

```
cd src/
python -m scripts.create_tables  # create schema
python -m scripts.etl            # option 1: load data one file per commit
python -m scripts.etl_bulk       # option 2: bulk copy for each table
```

Check results

```
jupyter notebook  # launch jupyter notebook app

# The notebook interface will appear in a new browser window or tab.
# Navigate to src/notebooks/test.ipynb and run the code cells
```
