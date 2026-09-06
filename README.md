# What Musical Features Make a Track Popular? 
### Karen Lie 
This is a project for DSC80 at UCSD

## Introduction
Music on Spotify varies in both its audio characteristics and its popularity. In this project, I investigate whether more danceable songs tend to be more popular and whether this relationship remains consistent across different genres. My main research question is: 
"Do tracks with higher danceability tend to have higher popularity, and does this relationship remain consistent when comparing tracks within individual genres?" 

The original dataset contains 114,000 Spotify tracks across 114 genres. For my analysis, I focus on 4,766 unique tracks from five musically distinct genres: pop, hip-hop, rock, country, and classical. The main columns relevant to my analysis are:  

danceability: How suitable a track is for dancing, from 0 to 1.
popularity: The track's Spotify popularity score.
track_genre: The genre of the track.
energy: How intense and energetic a track sounds, from 0 to 1.
valence: How positive or happy a track sounds, from 0 to 1.
tempo: The estimated speed of the track.

Genre is particularly important because different genres may naturally have different levels of both danceability and popularity. Therefore, genre might be a confounding variable. 

## Data Cleaning
I first restricted the dataset to 5 genres: pop, hip-hop, rock, country, and classical tracks. I selected these genres because they are each very different in musical styles and will allow me to compare whether the relationship between danceability and popularity changes across genres.

I removed rows with missing values in danceability, popularity, or track_genre, since these variables are necessary for my analysis. I also removed duplicate tracks based on track_id. After cleaning, the dataset contains 4,766 unique tracks. 

### Univariate Analysis
I first examined the distributions of danceability and popularity.

### (dist. danceability plot here)

Most tracks have danceability scores between approximately 0.4 and 0.8, with the distribution centered around 0.6.

### (dist. pop plot here)

Popularity has a noticeably different distribution. Many tracks have popularity scores close to 0, while there is another concentration of tracks with popularity scores around 60 to 80.

### Bivariate Analysis 
I examined the relationship between danceability and popularity.

### danceabilty x pop plot here 

The overall scatter plot does not show a very strong relationship between danceability and popularity. However, many tracks with higher popularity have moderate to high danceability. There are also many tracks with popularity close to 0 across a wide range of danceability scores.

Next, I separated this relationship by genre.

### by genre plot here 

The patterns differ across genres. For example, rock appears to show increasing popularity at higher danceability levels, while hip-hop shows a different pattern. There are also noticeable differences in both danceability and popularity between genres. This suggests that genre may be a confounding variable in the overall relationship between danceability and popularity.

### Interesting Aggregates 
Hip-hop has the highest average danceability with 0.736, while pop has the highest average popularity with 45.26. Classical has both the lowest average danceability (0.382) and the lowest average popularity (13.52) . I also separated tracks into four danceability levels and calculated average popularity within each genre. The relationship is not consistent across genres. For example, average popularity increases as danceability increases for rock, while it decreases across the observed danceability levels for hip-hop. This provides additional evidence that genre may confound the overall relationship.


