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

<iframe
  src="danceability.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Most tracks have danceability scores between approximately 0.4 and 0.8, with the distribution centered around 0.6.

<iframe
  src="popularity_distribution.html"
  width="100%"
  height="600"
  frameborder="0">
</iframe>

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

## Baseline Model
My baseline model is a decision tree classifier using two features, danceability (quantitative) and track_genre (nominal). Since track_genre is nominal, it is transformed using one-hot encoding. Danceability is left unchanged. The preprocessing and decision tree classifier are combined into a single sklearn Pipeline. I use a maximum tree depth of 3 for the baseline model. I split the data into training and test sets, using 80% for training and 20% for testing. The model achieved an F1-score of 0.000 on the test set. This means that the baseline model was unable to correctly identify popular tracks. Therefore, I do not consider the baseline model to perform well. One possible reason could be the class imbalance in the dataset and also the limited information provided by only 2 features, danceability and genre. 

## Final Model 
For my final model, I kept danceability and track_genre from the baseline model and added energy and valence. 

I transformed these 2 additional features. high_energy, which is energy is converted into a binary feature using a threshold of 0.6. This separates high-energy tracks from low-energy tracks. positive_valence, which is valence is converted into a binary feature using a threshold of 0.5. So it separates more positive-sounding tracks from less positive-sounding tracks. I selected these features because they describe different musical characteristics and adds information other than those in the baseline model. 

I continued using a decision tree classifier. To select its complexity, I tuned max_depth, which controls how deep the decision tree can grow. A tree that is too shallow may underfit the data, while a tree that is too deep may overfit.

I used GridSearchCV with 5-fold cross-validation and tested:
1, 2, 3, 4, 5, 6, 8, 10, 15, 20, 30, 40, and None

The best hyperparameter was max_depth = 20, and the best cross-validation F1-score was approximately 0.266. On the unseen test set, the final model achieved an F1-score of 0.228, compared with 0.000 for the baseline model. Therefore, the final model performs better than the baseline at identifying popular tracks. Adding information about energy and valence and tuning the decision tree improved its performance, although an F1-score of 0.228 indicates that there is still room for improvement.

## Fairness Analysis 
For fairness analysis, I wanted to investigate whether my final model performs similarly for pop and hip-hop tracks.

Group X: Pop tracks
Group Y: Hip-hop tracks
Evaluation Metric: Recall
Test Statistic: Recall for pop tracks minus recall for hip-hop tracks
Significance Level: 0.05

I chose recall because it measures the proportion of actually popular tracks that the model successfully identifies. This allows me to compare whether the model is equally successful at detecting popular songs in the two genres.

Null Hypothesis: The model has roughly the same recall for pop and hip-hop tracks, and any observed difference is due to random chance.

Alternative Hypothesis: The model has lower recall for hip-hop tracks than for pop tracks.

The observed difference in recall was approximately 0.338, with pop having the higher recall. I performed a permutation test by shuffling the genre labels 1,000 times and recalculating the difference in recall.

## add plot

The simulated p-value was 0.000, meaning none of the 1,000 permutations produced a difference as large as the observed difference.
Since the p-value is below 0.05, I reject the null hypothesis. The results provide evidence that the model performs worse at identifying popular hip-hop tracks than popular pop tracks. Therefore, according to recall, the final model does not perform equally across these two groups.
