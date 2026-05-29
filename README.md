# Predicting Spotify Track Popularity Using Audio Features

## Introduction

This project analyzes a Spotify tracks dataset containing over 100,000 songs and their associated audio features. The primary goal of this project is to investigate which characteristics of a song are associated with popularity and to build a predictive model that estimates a track's popularity score using its audio features.

Understanding the factors that contribute to popularity can provide insights into listener preferences and may be useful for artists, producers, and music recommendation systems. Additionally, examining relationships between audio characteristics and popularity can provide a better understanding of trends within modern music.

The dataset contains approximately 114,000 tracks and includes information about each song's audio characteristics and metadata.

Relevant variables used throughout this analysis include:

- **popularity**: Spotify popularity score ranging from 0 to 100.
- **danceability**: How suitable a track is for dancing.
- **energy**: Measure of intensity and activity.
- **loudness**: Overall loudness of the track in decibels.
- **valence**: Musical positivity conveyed by the track.
- **tempo**: Beats per minute.
- **explicit**: Whether the song contains explicit content.
- **track_genre**: Genre classification of the song.

---

# Data Cleaning

The dataset contained missing values in several columns. The columns `artists`, `album_name`, and `track_name` each contained a single missing value, while `tempo` contained over 22,000 missing values.

Since tempo was an important variable for later analyses and predictive modeling, missing tempo values were handled using median imputation within the modeling pipeline. The remaining missing values occurred in identifying metadata and therefore had minimal impact on the analyses.

To investigate patterns of missingness, a new boolean column called `tempo_missing` was created, indicating whether a tempo value was missing.

### Preview of Cleaned Dataset

Paste the output of:

```python
 track_id               | artists                | album_name                                             | track_name                 |   popularity |   duration_ms | release_date   | explicit   |   danceability |   energy |   key |   loudness |   mode |   speechiness |   acousticness |   instrumentalness |   liveness |   valence |   tempo |   time_signature | track_genre   |\n|:-----------------------|:-----------------------|:-------------------------------------------------------|:---------------------------|-------------:|--------------:|:---------------|:-----------|---------------:|---------:|------:|-----------:|-------:|--------------:|---------------:|-------------------:|-----------:|----------:|--------:|-----------------:|:--------------|\n| 5SuOikwiRyPMVoIQDJUgSV | Gen Hoshino            | Comedy                                                 | Comedy                     |           73 |        230666 | 1974           | False      |          0.676 |   0.461  |     1 |     -6.746 |      0 |        0.143  |         0.0322 |           1.01e-06 |     0.358  |     0.715 |  87.917 |                4 | acoustic      |\n| 4qPNDBW1i3p13qLCt0Ki3A | Ben Woodward           | Ghost (Acoustic)                                       | Ghost - Acoustic           |           55 |        149610 | 1995-04        | False      |          0.42  |   0.166  |     1 |    -17.235 |      1 |        0.0763 |         0.924  |           5.56e-06 |     0.101  |     0.267 |  77.489 |                4 | acoustic      |\n| 1iJBSr7s7jYXzM8EGcbK5b | Ingrid Michaelson;ZAYN | To Begin Again                                         | To Begin Again             |           57 |        210826 | 1973           | False      |          0.438 |   0.359  |     0 |     -9.734 |      1 |        0.0557 |         0.21   |           0        |     0.117  |     0.12  |  76.332 |                4 | acoustic      |\n| 6lfxq3CG4xtTiEg7opyCyx | Kina Grannis           | Crazy Rich Asians (Original Motion Picture Soundtrack) | Can't Help Falling In Love |           71 |        201933 | 2018-08-10     | False      |          0.266 |   0.0596 |     0 |    -18.515 |      1 |        0.0363 |         0.905  |           7.07e-05 |     0.132  |     0.143 | 181.74  |                3 | acoustic      |\n| 5vjLSffimiIP26QG5WcN2K | Chord Overstreet       | Hold On                                                | Hold On                    |           82 |        198853 | 2017-02-03     | False      |          0.618 |   0.443  |     2 |     -9.681 |      1 |        0.0526 |         0.469  |           0        |     0.0829 |     0.167 | nan     |                4 | acoustic      |
```

here.

---

# Univariate Analysis

## Distribution of Popularity

<iframe
    src="assets/popularity_histogram.html"
    width="100%"
    height="600"
    frameborder="0">
</iframe>

The popularity distribution is concentrated in the lower and middle popularity ranges, with relatively few tracks achieving extremely high popularity scores. This indicates that highly popular songs represent only a small fraction of the tracks available on Spotify.

The distribution is also somewhat right-skewed, suggesting that achieving very high popularity is relatively uncommon.

---

# Bivariate Analysis

## Danceability vs Popularity

**plot here**

Tracks with higher danceability generally tend to have somewhat higher popularity scores, although there is still substantial variation. This suggests that danceability may contribute to popularity but is not the sole factor determining a song's success.

Other factors outside of the available audio features likely influence popularity as well.

---

# Interesting Aggregate

## Average Popularity by Genre

Paste your grouped table here.

The aggregate analysis shows that average popularity differs considerably across genres. Some genres consistently achieve higher popularity scores than others, suggesting that genre may play an important role in determining audience reach and listener engagement.

These differences may reflect varying audience sizes, listening trends, and levels of commercial success across genres.

---

# Assessment of Missingness

## NMAR Analysis

The `tempo` column contains substantial missingness. It is possible that tempo is NMAR (Not Missing At Random) if songs with unusual rhythmic structures or difficult-to-detect tempos are more likely to have missing tempo values. However, without additional information regarding Spotify's tempo extraction process, it is difficult to determine whether the missingness is truly NMAR.

Additional metadata describing how tempo values were generated would help determine whether tempo missingness depends on unobserved factors.

## Missingness Dependency

To investigate missingness, a permutation test was performed to determine whether tempo missingness depended on other variables in the dataset.

The missingness rate varied substantially across genres. Genres such as romance, ambient, and new-age exhibited some of the highest proportions of missing tempo values. The permutation test produced a p-value near zero, providing strong evidence that tempo missingness depends on genre.

In contrast, missingness rates were relatively similar between explicit and non-explicit tracks. Explicit tracks had a missingness rate of approximately 16%, while non-explicit tracks had a missingness rate of approximately 20%. This difference was much smaller and provided little evidence that tempo missingness depends on explicitness.

**missingness plot**

These findings suggest that tempo missingness is likely MAR (Missing At Random) with respect to genre and does not appear to depend meaningfully on explicit content.

---

# Hypothesis Testing

## Research Question

Do explicit tracks tend to have higher popularity scores than non-explicit tracks?

### Null Hypothesis

Explicit and non-explicit tracks come from the same distribution of popularity scores. Any observed difference in average popularity is due to random chance.

### Alternative Hypothesis

Explicit tracks tend to have higher popularity scores than non-explicit tracks.

### Test Statistic

Difference in mean popularity between explicit and non-explicit tracks.

### Significance Level

α = 0.05

### Results

The observed difference in mean popularity between explicit and non-explicit tracks was approximately **3.52 popularity points**.

The permutation test produced a **p-value of approximately 0**.

### Conclusion

Because the p-value is substantially smaller than 0.05, the data provide strong evidence against the null hypothesis. These results suggest that explicit tracks tend to have higher popularity scores than non-explicit tracks.

<iframe
    src="assets/Permutation_Distribution.html"
    width="100%"
    height="600"
    frameborder="0">
</iframe>

---

# Prediction Problem

The goal of the prediction task is to predict a track's Spotify popularity score using its audio features and metadata.

This is a **regression problem** because popularity is a continuous numerical variable ranging from 0 to 100.

### Response Variable

- popularity

### Features Available at Prediction Time

The model uses information that would reasonably be available when a track is uploaded to Spotify:

- danceability
- energy
- loudness
- valence
- tempo
- explicit
- track_genre

### Evaluation Metric

The primary evaluation metric is **Root Mean Squared Error (RMSE)**.

RMSE was chosen because popularity is a continuous variable and RMSE measures prediction error in the same units as the target variable. It also penalizes larger errors more heavily, making it an appropriate metric for evaluating regression performance.

---

# Baseline Model

The baseline model was a Linear Regression model implemented using a scikit-learn Pipeline.

The model used several quantitative variables:

- danceability
- energy
- loudness
- valence
- tempo

The model also included two nominal categorical variables:

- explicit
- track_genre

Categorical variables were encoded using one-hot encoding while quantitative variables were standardized before training.

### Baseline Model Performance

- RMSE: **19.14**
- R²: **0.258**

The baseline model explains approximately 25.8% of the variation in popularity scores. While the model captures some meaningful relationships between audio features and popularity, a substantial amount of variation remains unexplained.

---

# Final Model

The final model used a Random Forest Regressor combined with feature engineering and hyperparameter tuning.

## Engineered Features

### Energy-Valence Interaction

A new feature was created by multiplying energy and valence.

This feature captures songs that are simultaneously energetic and positive. Songs with both characteristics may appeal differently to listeners than songs possessing only one of these attributes.

### Loudness-Energy Ratio

A second engineered feature was created using the ratio of loudness to energy.

This feature captures the relationship between perceived loudness and overall track intensity and may provide additional information beyond either feature individually.

## Hyperparameter Tuning

GridSearchCV was used to search over the following hyperparameters:

- n_estimators: [25, 50]
- max_depth: [10, 20]

These parameters were selected because they directly control model complexity. Increasing tree depth allows the model to learn more complex patterns, while increasing the number of trees may improve prediction stability.

### Best Hyperparameters

- max_depth = 20
- n_estimators = 50

### Final Model Performance

- RMSE: **20.46**
- R²: **0.170**

### Comparison to Baseline Model

The final model did not outperform the baseline model. Although additional engineered features and a more flexible algorithm were introduced, the Random Forest model produced a larger RMSE and a lower R² score than the baseline linear regression model.

This result suggests that the relationships between the selected features and popularity may be captured effectively by the simpler baseline model.

---

# Fairness Analysis

The fairness analysis investigated whether the final model performed differently on explicit and non-explicit tracks.

### Group X

Explicit tracks

### Group Y

Non-explicit tracks

### Evaluation Metric

RMSE

### Null Hypothesis

The model's RMSE is the same for explicit and non-explicit tracks. Any observed difference is due to random chance.

### Alternative Hypothesis

The model has a higher RMSE for explicit tracks than for non-explicit tracks.

### Test Statistic

Difference in RMSE between explicit and non-explicit tracks.

### Significance Level

α = 0.05

### Results

- RMSE (Explicit): **23.52**
- RMSE (Non-Explicit): **20.14**
- Observed Difference: **3.38**
- p-value: **0.063**

### Conclusion

The model produces a larger prediction error for explicit tracks than for non-explicit tracks. However, because the p-value exceeds the significance level of 0.05, there is insufficient statistical evidence to conclude that the model systematically performs worse for explicit tracks.

While some disparity is observed, the results do not provide strong evidence that the model is unfair with respect to explicitness.

[INSERT FAIRNESS PERMUTATION PLOT HERE]
