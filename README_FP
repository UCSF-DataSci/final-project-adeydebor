# What Makes a Song Go Viral?  
### Predicting Spotify Stream Performance Using Audio and Popularity Features

## Project Overview
This project explores whether we can predict a song streaming success using publicly available audio and engagement data. Using Spotify’s 2024 dataset of top-streamed songs, we aim to classify songs into two groups: those with above-median stream counts (“viral”) and those below.

While “virality” often includes subjective and multifaceted components (e.g., TikTok trends, playlist reach), for this project, **we define a song as "viral" if its Spotify stream count is above the median** in the dataset. 
## Dataset
Dataset: [`spotify_streams_2024.csv`](./data/spotify_streams_2024.csv)  
Source: [Kaggle – Most Streamed Spotify Songs 2024](https://www.kaggle.com/datasets/nelgiriyewithana/most-streamed-spotify-songs-2024)
- Sample Size: 700+ songs
- Key Variables:
  - Spotify Popularity Score
  - Playlist Count & Reach
  - YouTube Views & Likes
  - TikTok Posts & Likes
  - Apple Music + Shazam Counts
  - Explicit Track (Yes/No)

## Modeling
- Primary Model: Logistic Regression
- Optional Model: Random Forest

Target variable:
```python
df["viral"] = (df["Spotify Streams"] > df["Spotify Streams"].median()).astype(int)
