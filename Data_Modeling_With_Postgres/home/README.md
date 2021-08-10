# Sparkify song play logs ETL process

This ETL project is about to load logs and songs data from the Sparkify app , Below are the list of 5 tables which will record the Sparkify app data
 - `users`
 - `songs`
 - `artists`
 - `songplays`
 - `time` - Time table where start time is broken down to year , month, weekday, week 
 
This tables will help to analyze user habits and kind of the songs they play so that we can understand more our users and recommnend them new songs. 


## Running the ETL

First we need to create PostgreSQL database , by executing bewlow python file:

```
python create_tables.py
```

Then we execute below python etl file to perfrom the etl and load data in the respective tables:

```
python etl.py
```

## Database Schema Design

Below is the each table design and structure: 

### Song Plays table

- *Name:* `songplays`
- *Type:* Fact table

| Column | Type | Description |
| ------ | ---- | ----------- |
| `songplay_id` | `SERIAL NOT NULL` | Unique Identifier for Song_Plays Table | 
| `start_time` | `TIMESTAMP NOT NULL` | The timestamp that this song play log happened |
| `user_id` | `INTEGER REFERENCES users (user_id)` | The user id that triggered this song play log. It cannot be null, as we don't have song play logs without being triggered by an user.  |
| `level` | `VARCHAR NOT NULL` | The level of the user that triggered this song play log |
| `song_id` | `VARCHAR REFERENCES songs (song_id)` | The identification of the song that was played. It can be null.  |
| `artist_id` | `VARCHAR REFERENCES artists (artist_id)` | The identification of the artist of the song that was played. |
| `session_id` | `INTEGER NOT NULL` | The session_id of the user on the app |
| `location` | `VARCHAR` | The location where this song play log was triggered  |
| `user_agent` | `TEXT` | The user agent of our app |

### Users table

- *Name:* `users`
- *Type:* Dimension table

| Column | Type | Description |
| ------ | ---- | ----------- |
| `user_id` | `INTEGER CONSTRAINT users_pk PRIMARY KEY NOT NULL` | Unique Identifier for a user |
| `first_name` | `VARCHAR NOT NULL` | First name of the user |
| `last_name` | `VARCHAR NOT NULL` | Last name of the user. |
| `gender` | `CHAR(1)` | The gender is stated with just one character `M` (male) or `F` (female) |
| `level` | `VARCHAR NOT NULL` | The level stands for the user app plans (`premium` or `free`) |



### Songs table

- *Name:* `songs`
- *Type:* Dimension table

| Column | Type | Description |
| ------ | ---- | ----------- |
| `song_id` | `VARCHAR  CONSTRAINT songs_pk PRIMARY KEY NOT NULL` | Unique Identfier for a song | 
| `title` | `VARCHAR NOT NULL` | Title of the Song. |
| `artist_id` | `VARCHAR REFERENCES artists (artist_id)` |Artist Identifier |
| `year` | `INTEGER` | The year that this song was made |
| `duration` | `FLOAT` | The duration of the song |




### Artists table

- *Name:* `artists`
- *Type:* Dimension table

| Column | Type | Description |
| ------ | ---- | ----------- |
| `artist_id` | `VARCHAR CONSTRAINT artist_pk PRIMARY KEY NOT NULL` | Unique Identifier of an artist |
| `name` | `VARCHAR NOT NULL` | The name of the artist |
| `location` | `VARCHAR` | Location of artist |
| `latitude` | `DECIMAL(9,6)` | The latitude of the location |
| `longitude` | `DECIMAL(9,6)` | The longitude of the location |





### Time table

- *Name:* `time`
- *Type:* Dimension table

| Column | Type | Description |
| ------ | ---- | ----------- |
| `start_time` | `TIMESTAMP CONSTRAINT start_time_pk PRIMARY KEY` | Song Start Time  |
| `hour` | `INTEGER NOT NULL` | The hour from the timestamp  |
| `day` | `INTEGER NOT NULL` | The day of the month from the timestamp |
| `week` | `INTEGER NOT NULL` | The week of the year from the timestamp |
| `month` | `INTEGER NOT NULL` | The month of the year from the timestamp |
| `year` | `INTEGER NOT NULL` | The year from the timestamp |
| `weekday` | `VARCHAR NOT NULL` | The week day from the timestamp |




## The project file structure

We have a small list of files, easy to maintain and understand:
 - `sql_queries.py` - Files consist of all the sql script required to create ,load , drop tables and query lists which are being used in etl.py & create_tables.py
 - `create_tables.py` -Python code to create  sparkifydb and tables in Postgres
 - `etl.py` - It's perform main ETL process
 - `etl.ipynb` - The python notebook to test the logic behind the `etl.py` process
 - `test.ipynb` - Test python Notebook to query sparkifydb & tables to see if the etl is working fine or not.