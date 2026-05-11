# Airbnb Madrid — Sentiment Analysis & Review Score Prediction
## Project Overview
**Tools:** RapidMiner (AI Studio), Excel  

**Skills:** Sentiment Analysis, Text Mining, Multiple Linear Regression, Correlation Analysis, Outlier Detection, Cross-Validation

**Author:** Quang Huy Le
 
This project was developed for Airbnb to analyse customer feedback and predict review scores for Madrid rental properties. Two analytical pipelines were built: one to examine the correlation between host descriptions and customer sentiment, and another to predict overall review ratings using key property and guest experience attributes.
 
## Tasks
### Task A — Sentiment Score Correlation (Text Mining)
 
**Objective:** Determine whether a significant correlation exists between the sentiment expressed in host property descriptions and customer review comments.
 
**Pipeline:**
1. Filter missing host descriptions (1,138 removed)
2. Text preprocessing: lowercase → tokenise → stem → remove stopwords → filter tokens (4–25 chars)
3. Match against positive/negative word lists to generate sentiment scores
4. Join host and customer datasets on property ID
5. Correlation Matrix applied to compare sentiment scores
**Result:** Correlation coefficient = **0.183** (weak positive correlation)
 
**Interpretation:** A slight tendency exists for positive host descriptions to align with positive customer reviews. However, the weak correlation suggests customers are more influenced by their actual experience than by listing descriptions.
 
### Task B — Review Score Prediction (Multiple Linear Regression)
 
**Objective:** Build a predictive model to estimate the overall `review_scores_rating` of Airbnb properties.
 
#### Data Preparation
- Selected 12 candidate attributes based on domain relevance
- Replaced missing bedroom values with 0 (assumed studio apartments)
- Removed remaining missing values via Filter Examples
- Applied Nominal to Numerical conversion for correlation analysis
- Used Weight by Correlation to rank predictors
**Top Predictors by Correlation Weight:**
| Attribute | Weight |
|---|---|
| review_scores_value | 0.816 |
| review_scores_accuracy | 0.785 |
| review_scores_cleanliness | 0.736 |
| review_scores_communication | 0.695 |
| review_scores_checkin | 0.651 |
| review_scores_location | 0.474 |
 
#### Modelling
- Outlier detection via k-NN Global Anomaly Score (k=10); extreme outliers (score > 6) removed
- Z-transformation normalisation applied
- 70/30 train-test split (Hold-out method)
- Linear Regression with M5 Prime feature selection (tolerance threshold = 0.05)
- Cross-validation (10-fold) also applied for comparison
#### Final Model Coefficients
| Attribute | Coefficient | Interpretation |
|---|---|---|
| review_scores_value | 3.943 | Strongest driver — +3.943 pts per unit increase |
| review_scores_accuracy | 2.379 | +2.379 pts per unit increase |
| review_scores_cleanliness | 1.986 | +1.986 pts per unit increase |
| review_scores_communication | 1.519 | +1.519 pts per unit increase |
| review_scores_checkin | 0.621 | +0.621 pts per unit increase |
| review_scores_location | -0.222 | Slight negative effect (high-demand trade-offs) |
| Intercept | 92.583 | Baseline score |
 
All predictors significant (p < 0.05), no multicollinearity (tolerance > 0.1 for all).
 
## Model Performance
 
| Method | RMSE | Squared Correlation |
|---|---|---|
| Hold-out (70/30) | 4.386 | 0.804 |
| Cross-validation (10-fold) | 4.625 ± 0.183 | 0.780 ± 0.014 |
 
**Interpretation:** The model explains ~80% of variability in review scores. Hold-out performs slightly better on the specific test set, while cross-validation provides a more robust and generalisable estimate. Both methods show the model performs better for low-to-moderate scores and underpredicts very high ratings (near 100).
 
## Key Findings & Recommendations
 
- **Value for money** is the strongest predictor of guest satisfaction — hosts should ensure competitive and fair pricing.
- **Accuracy** is the second most important factor — listing descriptions must closely reflect the actual property.
- **Cleanliness and communication** are highly valued by guests and should be maintained consistently.
- **Location** has a slight negative coefficient, likely due to trade-offs in high-demand areas (higher prices, overcrowding).
- To improve model performance, consider non-linear models and encoding categorical variables (e.g. property type, neighbourhood).

