# What Makes a Song Go Viral?  
### An Exploratory Analysis of Spotify Streaming Success in 2024

## Project Overview
This project explores what characteristics are associated with a song’s success on Spotify in 2024. Songs are classified as “viral” or “non-viral” based on whether their stream count is above or below the dataset’s median.

While “virality” often includes subjective and multifaceted components (e.g., TikTok trends, playlist reach), for this project, **we define a song as "viral" if its Spotify stream count is above the median** in the dataset. 
## Dataset
Dataset: [`spotify_streams_2024.csv`](./data/spotify_streams_2024.csv)  
Source: [Kaggle – Most Streamed Spotify Songs 2024](https://www.kaggle.com/datasets/nelgiriyewithana/most-streamed-spotify-songs-2024)
- Sample Size: 4600+ songs
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
