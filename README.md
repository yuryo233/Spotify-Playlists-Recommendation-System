# Spotify-Playlists-Recommendation-System
recommend songs to Spotify playlists by collaborative-filtering and clustering models in PySpark

This project is the final project of the Mining of Large Datasets course.

## 1.	Introduction
The goal of this project is to recommend new songs to every playlist in the dataset by building Collaborative Filtering and Clustering models. The dataset is obtained from Spotify Million Playlist Dataset Challenge(https://www.aicrowd.com/challenges/spotify-million-playlist-dataset-challenge) which contains 1,000,000 playlists created by Spotify users from January 2010 to October 2017. In this project, we randomly select 20 files out of total 1000 files as subset, and each file contains 1000 playlists. 

## 2.	Initial impression
In the first task, we would like to find basic information of this dataset including:
- number of playlists: 20,000
- number of tracks: 1,328,284
- number of unique tracks: 268,907
- number of unique albums: 123,132
- number of unique artists: 51,826
- number of unique titles: 10,140
- number of playlists with descriptions: 364
- avg playlist length in minute: 259.74

## 3.	Collaborative Filtering
-	**Item to item recommendation**\
To improve the modeling efficiency, we would like to build models on one file first. The first model is simply item to item recommendation system. The primary task is to create a similarity matrix for each song in the dataset, so that the song with the largest sum of similarity coefficients of the songs contained in the playlist is the top recommendation. We calculated the similarity coefficients by Jaccard index with sets of playlists that contain speciic songs.

-	**ALS models**\
Continue with the one file dataset, we sought to build ALS models based on implicit ratings. We have built two models with different settings of Rating. Model 1 is created on an RDD of Rating(playlist ID, track ID, rating) tuple where Rating is 1 if track in playlist or 0 otherwise. Model 2 is created on an RDD of Rating(playlist ID, artist ID, rating) tuple where Rating is count of artist in playlist, then all 20 files are loaded for Model 2. Based on the optimal model setting with minimized MSE, we could recommend top artists to a playlist, then we imported Spotipy library to generate top tracks of these artists.

## 4.	Music Genres Clustering
In this task, we used one file of playlist dataset and one file of audio data of all the tracks in the dataset. The audio data is obtained from Spotify API(https://developer.spotify.com/documentation/web-api/) which contains **track_id, acousticness, danceability, duration_ms, energy, instrumentalness, key, liveness, loudness, mode, speechiness, tempo and valence**. Then we would like to use k-means clustering to classify these tracks into k groups, and observe the features of each group to define music genres. We expected the number of music genres in this dataset is 6, so k is set to 6. Based on the clustering centers obtained from the model, we can define these genres: **chill as acoustic and sad, rock as loud, classical as instrumental and live, pop as everything else, hip-hop as speechy, and r-n-b as quieter than pop**. As we have labeled each track with specific genre, we can define playlist genre by the most frequent track genre. Finally, we would like to use Spotipy to generate top songs in the same genre.

## 5.	Conclusion
Due to computational limits, only ALS model 2 can be built on all 20 files. Besides, item to item recommendation and ALS model 1 are restricted to the tracks contained in the dataset, so the results lack dynamic of music. The clustering model can recommend songs from Spotify API, while the results are specific to genres rather than playlists. Therefore, **ALS model 2 is an appropriate method to recommend songs to playlists** in this project for its capability of dealing with large datasets and generating desirable results.

## 6.	References
1. https://www.aicrowd.com/challenges/spotify-million-playlist-dataset-challenge
2. https://spark.apache.org/docs/latest/ml-collaborative-filtering.html
3. https://github.com/vaslnk/Spotify-Song-Recommendation-ML/blob/master/EDA.ipynb
4. https://developer.spotify.com/documentation/web-api/
5. https://www.kaylinpavlik.com/classifying-songs-genres/
