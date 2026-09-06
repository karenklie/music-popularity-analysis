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


## Assessment of Missingness
Within the five selected genres, tempo is the only column with missing values. There are 1,100 tracks with missing tempo values and 3,900 tracks with recorded tempo values in the original five-genre subset. Tracks with missing tempo differ from tracks with recorded tempo on several observed characteristics. For example, tracks with missing tempo have an average energy of approximately 0.439, compared with 0.582 for tracks with recorded tempo.

### Missingness Mechanism 
It is possible that tempo could be NMAR if the reason a tempo value is missing depends on the true tempo itself. For example, certain tempos could potentially be more difficult to identify. Additional information about why Spotify's audio analysis did not record a tempo for a particular track could help explain the missingness and potentially make the mechanism MAR.

I performed permutation tests to determine whether tempo missingness depends on observed features.

Null Hypothesis: Tempo missingness does not depend on the feature being tested. Any observed difference is due to random chance.
Alternative Hypothesis: Tempo missingness depends on the feature being tested.
I used the absolute difference in group means as my test statistic and a significance level of 0.05.

**Energy**
For energy, the simulated p-value was 0.000. None of the 1,000 permutations produced a difference at least as large as the observed difference.
Therefore, I reject the null hypothesis. There is strong evidence that tempo missingness is associated with a track's energy.

**Track Duration**
I repeated the test using duration_ms. This test produced a simulated p-value of approximately 0.98. Since the p-value is much greater than 0.05, I fail to reject the null hypothesis. There is not sufficient evidence that tempo missingness depends on track duration.

Overall, tempo missingness depends on at least one observed variable, energy, but not every variable tested. These results are consistent with tempo being MAR rather than MCAR.

## Hypothesis Testing 
I next tested whether tracks with higher danceability tend to have higher popularity. I divided tracks into high- and low-danceability groups using the median danceability score.

Null Hypothesis: High-danceability and low-danceability tracks have the same average popularity. Any observed difference is due to random chance.
Alternative Hypothesis: High-danceability tracks have higher average popularity than low-danceability tracks.
Test Statistic: Mean popularity of high-danceability tracks minus mean popularity of low-danceability tracks.
Significance Level: 0.05

I chose the difference in mean popularity because I am comparing the average popularity of two groups. I used a one-sided test because my alternative hypothesis specifically asks whether the high-danceability group has higher average popularity. High-danceability tracks had an average popularity of approximately 32.36, while low-danceability tracks had an average popularity of approximately 21.38. The observed difference was therefore approximately 10.98 popularity points.

### plot

The simulated p-value was 0.000, meaning none of the 1,000 permutations produced a difference as large as the observed difference. Since the p-value is below 0.05, I reject the null hypothesis. There is strong evidence that tracks in the high-danceability group have higher average popularity than tracks in the low-danceability group. However, this test considers all genres together. My earlier analysis showed that the relationship differs within individual genres, suggesting that genre may confound part of this overall association.

## Framing a Prediction Problem
For the prediction portion of my project, I predict whether a track is popular, where a popular track is defined as having a Spotify popularity score of at least 70. The response variable, is_popular, has two possible outcomes: popular or not popular. Therefore, this is a binary classification problem.

Of the cleaned tracks, approximately 14.4% are popular and 85.6% are not popular. This means the classes are imbalanced. A classifier that simply predicts that every track is not popular could already achieve approximately 85.6% accuracy, so accuracy alone would not be a useful evaluation metric. Instead, I will use F1-score, which considers both precision and recall for the positive class. This better evaluates how successfully the model identifies popular tracks. I did not use the original popularity column because is_popular is directly created from it, which would cause data leakage.

