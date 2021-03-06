## Project Summary
A Music Streaming Startup would like to get insight of the users activity on their music streaming app and get data insights to improve their services .
This project focuses on Extracting-Transforming-Loading (ETL) process and data modeling for the songs data in a star schema. The project will allow 
to help as well to bring answers to business questions (i.e, most listened songs etc.).

### Datasets

#### Songs dataset
Each file on the path `data/song_data` will contain the following JSON object:

```javascript
{
    "num_songs": 1,
    "artist_id": "AR7G5I41187FB4CE6C",
    "artist_latitude": null,
    "artist_longitude": null,
    "artist_location": "London, England",
    "artist_name": "Adam Ant",
    "song_id": "SONHOTT12A8C13493C",
    "title": "Something Girls",
    "duration": 233.40363,
    "year": 1982
}

```
#### Logs dataset
Each file on the path `data/log_data` will contain a list of the following object:

```javascript
{
    "artist": null,
    "auth": "Logged In",
    "firstName": "Walter",
    "gender": "M",
    "itemInSession": 0,
    "lastName": "Frye",
    "length": null,
    "level": "free",
    "location": "San Francisco-Oakland-Hayward, CA",
    "method": "GET",
    "page": "Home",
    "registration": 1540919166796.0,
    "sessionId": 38,
    "song": null,
    "status": 200,
    "ts": 1541105830796,
    "userAgent": "\"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.143 Safari/537.36\"",
    "userId": "39"
}
```

Sample Match Record (Two datasets combined) :
```
{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}
```

## Database Star Schema 

### Fact Table
1. songplays - records in log data associated with song plays i.e. records with page NextSong
    - songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

### Dimension Tables
2. users - users in the app
    -user_id, first_name, last_name, gender, level

3. songs - songs in music database
    - song_id, title, artist_id, year, duration

4. artists - artists in music database
    - artist_id, name, location, latitude, longitude

5. time - timestamps of records in songplays broken down into specific units
    - start_time, hour, day, week, month, year, weekday


### Database Tables

#### Table Artists
```sql
artists (
	artist_id VARCHAR PRIMARY KEY, 
    name VARCHAR, 
    location VARCHAR, 
    lattitude VARCHAR, 
    longitude VARCHAR
);
```
#### Table Songplays
```sql
songplays (
    songplay_id SERIAL PRIMARY KEY, 
    start_time BIGINT NOT NULL,
    user_id INT NOT NULL, 
    level VARCHAR, 
    song_id VARCHAR, 
    artist_id VARCHAR, 
    session_id INT, 
    location VARCHAR, 
    user_agent VARCHAR,
);
```
#### Table Songs
```sql
songs (
  song_id VARCHAR PRIMARY KEY, 
  title VARCHAR, 
  artist_id VARCHAR NOT NULL, 
  year INT, 
  duration NUMERIC
);
```
#### Table Time
```sql
time (
    start_time BIGINT PRIMARY KEY,
    hour INT, 
    day INT, 
    week INT, 
    month INT, 
    year INT, 
    weekday INT
);

```
#### Table Users
```sql
users (
    user_id INT PRIMARY KEY, 
    first_name VARCHAR, 
    last_name VARCHAR,
    gender VARCHAR, 
    level VARCHAR
);
```

## Development
The project was developed in 4 main steps :

1. Datasets understanding: It is important understand the logs of user activity, and the songs catalog. This will allow to design the star schema design
2. Design of the ETL: This includes the creation of the tables (fact and dimension) and their contraints
3. ETL design by cleaning and transforming teh data, the details of the transformations can be found at the etl.ipynb and the 
sql_queries.py to create the tables and database. 
4. Have a star schema, where we can start to get some data insights.

## Example Business Questions
After having the star schema, we can now answer some business questions, please notice that here I cover few examples, but a data analyst may found more useful data insights.

- Example query to combine all the 5 tables:

```sql
   SELECT * \
   FROM songplays JOIN songs ON songplays.song_id = songs.song_id \
   JOIN artists ON songplays.artist_id = artists.artist_id \
   JOIN time ON songplays.start_time =  time.start_time \
   JOIN users ON songplays.user_id = users.user_id \
   LIMIT 5;
   ```

- How many male and female users are in the platform?

```sql
   SELECT count(user_id), gender FROM users GROUP BY gender;
   ```

- User distribution between paid subscription and free

```sql
   SELECT count(user_id), gender, level FROM users as U GROUP BY gender, U.level;
   ```

- What are the top user's listeners

```sql
SELECT  users.first_name, songplays.user_id, count(songplays.user_id) 
FROM songplays 
JOIN users ON songplays.user_id = users.user_id 
GROUP BY songplays.user_id, users.first_name 
ORDER BY count(songplays.user_id) DESC;
```


## Project Structure
    .
    ????????? create_tables.py # drops and creates your tables. You run this file to reset your tables before each time you run your ETL scripts.
    ????????? etl.ipynb        # reads and processes a single file from song_data and log_data and loads the data into your tables. This notebook contains detailed instructions on the ETL process for each of the tables.           
    ????????? etl.py           # reads and processes files from song_data and log_data and loads them into your tables. You can fill this out based on your work in the ETL notebook.
    ????????? sql_queries.py   # contains all the sql queries for the project  
    ????????? test.ipynb       # Displays the first few rows of each table to let you check your database.

## How to run
To Extract - Transform - Load the data in the database please run the two scripts in order:

1. create_tables.py
2. etl.py

## Project Requirements

* Python3 with an [Anaconda Distribution](https://www.anaconda.com/products/individual) (recommended)
* Local or Cloud Postgres Database
* Songs dataset is a subset of [Million Song Dataset](http://millionsongdataset.com/).
