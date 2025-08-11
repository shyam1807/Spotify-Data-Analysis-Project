-- Advance SQL Project -- Spotify Database
-- create table


CREATE TABLE spotify1 (
    artist VARCHAR(255),
    track VARCHAR(255),
    album VARCHAR(255),
    album_type VARCHAR(50),
    danceability FLOAT,
    energy FLOAT,
    loudness FLOAT,
    speechiness FLOAT,
    acousticness FLOAT,
    instrumentalness FLOAT,
    liveness FLOAT,
    valence FLOAT,
    tempo FLOAT,
    duration_min FLOAT,
    title VARCHAR(255),
    channel VARCHAR(255),
    views FLOAT,
    likes FLOAT,
    comments FLOAT,
    licensed BOOLEAN,
    official_video BOOLEAN,
    stream BIGINT,
    energy_liveness FLOAT,
    most_played_on VARCHAR(50)
);

-- EDA


SELECT COUNT(*)  FROM spotify1;

SELECT COUNT(DISTINCT artist)  FROM spotify1;

SELECT DISTINCT album_type  FROM spotify1;

SELECT  MAX(duration_min)  FROM spotify1;

SELECT  Min(duration_min)  FROM spotify1;

SELECT * FROM spotify1
WHERE duration_min = 0

DELETE FROM spotify1
WHERE duration_min = 0;
SELECT * FROM spotify1
WHERE duration_min = 0;


SELECT DISTINCT channel FROM spotify1;

SELECT DISTINCT most_played_on FROM spotify1;

-- Data Analysis Easy Category


1. Retrieve the names of all tracks that have more than 1 billion streams.


SELECT * FROM spotify1
WHERE stream > 1000000000;



2.List all albums along with their respective artists.


SELECT 
      DISTINCT album, artist
FROM spotify1
ORDER BY 1;

SELECT 
      DISTINCT album
FROM spotify1
ORDER BY 1;



3.Get the total number of comments for tracks where licensed = TRUE.

 -- SELECT DISTINCT licensed  FROM spotify1

SELECT 
   SUM(comments) as total_comments
FROM spotify1
WHERE licensed = 'true';




4.Find all tracks that belong to the album type single.

SELECT * FROM spotify1
WHERE album_type = 'single'



5.Count the total number of tracks by each artist.


SELECT 
    artist, --- 1
	count(*) as total_no_song   ---2
FROM spotify1
GROUP BY artist
ORDER BY 2 

/*
----------------------------
Medium LEVEL 
----------------------------
*/


6.Calculate the average danceability of tracks in each album.

SELECT 
     album,
	 avg(danceability) as avg_danceability
	 FROM spotify1
	 GROUP BY 1
	 ORDER BY 2 DESC
	 


7.Find the top 5 tracks with the highest energy values.


SELECT
      track,
	  avg(energy)
FROM spotify1
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5




8.List all tracks along with their views and likes where official_video = TRUE.


SELECT
     track,
	  SUM(views) as total_views,
	  SUM(likes) as total_likes
FROM spotify1
 WHERE official_video = 'true'
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5



9.For each album, calculate the total views of all associated tracks.


SELECT 
      album,
	  track,
	  SUM(views)
	  FROM spotify1
	  GROUP BY 1,2
	  ORDER BY 3 DESC



10.Retrieve the track names that have been streamed on Spotify more than YouTube.

SELECT * FROM 
(SELECT 
     track,
	 --most_played_on,
	 COALESCE(SUM(CASE WHEN most_played_on = 'Youtube' THEN stream END),0)as streamed_on_youtube,
	 COALESCE(SUM(CASE WHEN most_played_on = 'Spotify' THEN stream END),0) as streamed_on_spotify
	 
FROM spotify1
GROUP BY 1
 )as t1
 WHERE 
     streamed_on_spotify > streamed_on_youtube
	 AND 
	 streamed_on_youtube <> 0


-- ----------------------
-- Advanced Problem 
-- ----------------------


11.Find the top 3 most-viewed tracks for each artist using window functions.

-- each artist and total view for each track
-- track with highest view for each artist(we need top)
-- dense rank 
-- cte and filder rank<=3
WITH ranking_artist
AS 
(SELECT
     artist,
	 track,
	 SUM(views) as total_view,
	 DENSE_RANK() OVER(PARTITION BY artist  ORDER BY SUM(views)DESC) as rank
	
FROM spotify1
GROUP BY 1,2
ORDER BY 1,3 DESC
)
 SELECT * FROM ranking_artist
 WHERE rank <=3






12.Write a query to find tracks where the liveness score is above the average.

SELECT 
      track,
	  artist,
	  liveness
	  FROM spotify1
	  WHERE liveness > (SELECT AVG(liveness) FROM spotify1)



13.Use a WITH clause to calculate the difference between the highest and lowest energy values for tracks in each album.


WITH cte
AS
(SELECT 
     album,
	 MAX(energy) as highest_energy,
	 Min(energy) as lowest_energy
	 FROM spotify1
	 GROUP BY 1
	 ) 
	 SELECT
	      album,
		  highest_energy - lowest_energy as energy_diff
		  from cte
		  ORDER BY 2 DESC
