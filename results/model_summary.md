# Model Summary

The models predict a property's recorded closing price (`ClosePrice`). Mean absolute error (MAE) gives the average size of a prediction error in dollars. Root mean squared error (RMSE) gives more weight to large errors.

## Predicting May closing prices for single-family homes

I trained three models on 49,703 single-family sales from December 2025 through April 2026 and tested them on 12,024 May 2026 sales. Linear regression had the lowest MAE and RMSE and the highest R² on this holdout. [Three-model comparison](../notebooks/04_model_comparison.ipynb)

| Model | Test MAE (USD) | Test RMSE (USD) | Test R² |
| --- | ---: | ---: | ---: |
| Linear regression | 437,938 | 1,460,357 | 0.243 |
| Random forest | 573,829 | 1,602,339 | 0.088 |
| Decision tree | 460,156 | 3,075,981 | −2.360 |

## Testing the value of three additional features

In a separate experiment, I used 23,260 May sales across property types. I added property age, bedroom-to-bathroom ratio, and school district to the original 14 predictors. Both feature sets used the same random split of 18,608 training sales and 4,652 test sales.

| Model | Original MAE (USD) | Expanded MAE (USD) | Original RMSE (USD) | Expanded RMSE (USD) |
| --- | ---: | ---: | ---: | ---: |
| Linear regression | 125,891 | 160,193 | 340,044 | 405,244 |
| Random forest | 68,621 | 64,656 | 786,632 | 526,022 |

Adding the features reduced random forest RMSE by **33.1%** and MAE by **5.8%**. Linear regression's RMSE increased by **19.2%**. The expanded random forest had the lowest average dollar error, while linear regression with the original features had the lowest RMSE. [Feature comparison](../notebooks/Week6_feature_engineering.ipynb)

## Where XGBoost predictions broke down

On the 4,652-sale May test set, tuned XGBoost had a **median absolute percentage error (MdAPE) of 9.81%**. Two predictions accounted for about **95% of total squared error**, driving test RMSE to **$7.96 million** and test R² to **−41.48**. [XGBoost results](../notebooks/05_advanced_models.ipynb)

| Test sales grouped by actual closing price | MdAPE |
| --- | ---: |
| Lowest third | 498.72% |
| Middle third | 7.96% |
| Highest third | 4.45% |

The lowest-priced third had the largest typical percentage error. Recorded sale prices in this group went as low as $2, so even a small dollar error can produce a large percentage error. [Evaluation by price band](../notebooks/06_evaluation_updated.ipynb)
