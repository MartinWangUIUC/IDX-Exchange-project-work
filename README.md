# IDX Exchange real estate valuation

**Martin (Hanran) Wang · Data Science Intern, IDX Exchange**

I used MLS sales records to study how well listing information predicts a property's closing price. I tested how three models performed on a later month of sales, then used a separate May sample to measure what changed when I added property age, the bedroom-to-bathroom ratio, and school district.

## Predicting closing prices for a later month

I combined **124,404 sales records** from December 2025 through May 2026 and identified **61,727 residential single-family sales**. I trained three models on 49,703 sales from December through April and evaluated them on the remaining 12,024 May sales. Linear regression had the lowest test root mean squared error (RMSE) and the highest test R² (0.243), ahead of the decision tree and random forest.

## Did the additional features improve predictions?

For this experiment, I used 23,260 May sales across property types. I compared the original 14 predictors, which included list price, with an expanded set that added property age, bedroom-to-bathroom ratio, and school district. All four fits used the same random split of 18,608 training sales and 4,652 test sales.

| Model | Test RMSE with original features | Test RMSE with added features | Change in RMSE | Change in mean absolute error |
| --- | ---: | ---: | ---: | ---: |
| Linear regression | $340,044 | $405,244 | +19.2% | +27.2% |
| Random forest | $786,632 | $526,022 | −33.1% | −5.8% |

Adding the features reduced both measures of random forest error. Linear regression with the original predictors still had the lowest RMSE of the four combinations.

## Where did predictions break down?

A tuned XGBoost model had a median absolute percentage error of **9.81%** on the May test sample. Two extreme predictions accounted for about **95% of the total squared error**, driving test R² to **−41.48**. Breaking the test set into price bands showed that the lowest-priced third had the largest typical percentage error.

## Notebook guide

| Analysis | Notebooks in run order |
| --- | --- |
| Six-month single-family analysis | [01 · Explore sales](notebooks/01_exploration.ipynb) → [02 · Prepare the chronological split](notebooks/02_preprocessing.ipynb) → [03 · Fit the linear baseline](notebooks/03_baseline_model.ipynb) or [04 · Compare three models](notebooks/04_model_comparison.ipynb) |
| May feature experiment | [Week 6 · Compare original and expanded features](notebooks/Week6_feature_engineering.ipynb) → [05 · Tune XGBoost](notebooks/05_advanced_models.ipynb) → [06 · Evaluate errors by price band](notebooks/06_evaluation_updated.ipynb) or [07 · Generate the Streamlit demo](notebooks/07_streamlit_app.ipynb) |

## Run locally

You can view the saved tables and plots on GitHub. Rerunning the models requires local copies of the MLS files, which are kept outside the public repository. From the repository root, install the Python packages and start JupyterLab.

```bash
python -m pip install jupyterlab pandas numpy scipy matplotlib scikit-learn xgboost joblib streamlit
jupyter lab
```

1. **Monthly sales.** Place the six `CRMLSSoldYYYYMM.csv` files in `data/`. Run notebook 01 while this folder contains only the monthly files, because it reads every CSV there. Notebook 02 writes the cleaned data used by 03 and 04.
2. **May feature experiment.** Place `enriched_data_with_school_district.csv` in `data/` and run the Week 6 notebook. It reads the district labels from this file and writes the engineered dataset to `results/Updated notebook + table comparing/feature_engineered_data.csv`. In notebook 05, point `data_path` to that output. Its default folder name includes an extra `Week 6` prefix.
3. **Evaluation and demo.** After 05, run 06 to examine the saved predictions or 07 to generate a local app. Launch the app with `streamlit run results/09/app.py`.
