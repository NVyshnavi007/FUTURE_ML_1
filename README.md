**Sales Forecasting — Future Interns ML Task 1**

Predicting monthly sales revenue for a retail business using historical order data, to support inventory, staffing, and cash flow planning.

**Problem Statement**

Retail businesses need to know not just what happened last quarter, but what's coming next. This project builds a time series forecasting model that predicts sales for the next 6 months based on historical order-level data, so decision-makers can plan ahead instead of reacting after the fact.

**Dataset**
Source: Sample Superstore dataset
Size: 9,994 order-line-item records
Time range: 2014–2017
Key columns used: Order Date, Sales

The raw data is at the order-line-item level (one row per product per order). For forecasting, it was aggregated into monthly total sales, giving ~48 monthly data points.

**Approach**
1. Data Cleaning & Exploration
Checked for missing values and duplicates (none found)
Reviewed summary statistics for all numeric columns
Identified outliers using the IQR method
2. Outlier Handling

Flagged outliers (e.g., orders over $10,000) were investigated individually rather than dropped automatically. They turned out to be legitimate large B2B purchases (enterprise copiers, videoconferencing systems) — not data errors — so they were kept in the dataset. Removing them would have hidden real demand spikes from the model.

3. Feature Engineering

Data was aggregated to monthly totals, then the following features were built:

t — a sequential time index capturing overall growth trend
Month, Quarter — calendar features for seasonality
Month_sin, Month_cos — cyclical (sin/cos) encoding of month, so December and January are treated as adjacent rather than 11 months apart
4. Train/Test Split

An 80/20 time-based split was used — training on the earliest months, testing on the most recent ones. Random shuffling was deliberately avoided, since that would let the model "see the future" during training and give a misleadingly optimistic test score.

5. Modeling

Two models were trained and compared:

Model	MAE	RMSE	MAPE
Linear Regression	—	—	20.5%
Random Forest	—	—	19.7%
6. Model Selection

Despite Random Forest scoring marginally lower on test MAPE, Linear Regression was selected as the final model. Reasoning:

The MAPE gap (0.8 points) is small enough to fall within normal variation given the limited number of test months (~10) — it isn't strong evidence RF is actually better.
Tree-based models like Random Forest cannot extrapolate a trend beyond the range of values they were trained on — they predict by repeating patterns from training data, not continuing them. For a multi-month forward forecast, this risks understating real growth.
Linear Regression correctly continues the upward trend, making it more reliable for actual forward-looking business planning.
7. Forecast

The final model was used to project sales for the next 6 months, visualized alongside historical actuals.
**Results & Business Interpretation**
The forecast shows sales trending upward toward Q4, consistent with typical year-end/holiday retail patterns.
Average forecast error (MAPE): ~20.5% — good enough to guide directional planning (inventory, staffing, cash flow timing), but should be treated as an estimate rather than an exact figure.

How a business can use this:

Inventory — order stock ahead of forecasted high-demand months to avoid stockouts
Cash flow — treat lower-forecast months as a signal to hold back non-essential spending
Staffing — bring on seasonal/temporary staff ahead of predicted peaks, not during them
**Limitations**
Only ~48 monthly data points after aggregation — a relatively small sample for model training/evaluation
The model assumes future business conditions resemble the 2014–2017 training period; major changes (new products, pricing shifts, market disruptions) would require retraining
Forecast should be used as a directional guide, not a precise number
**Tech Stack**
Python, pandas, numpy
scikit-learn (LinearRegression, RandomForestRegressor)
matplotlib
**Files**
Sample - Superstore.csv — raw dataset
notes.ipynb — full analysis notebook (cleaning, EDA, feature engineering, modeling, forecasting)
