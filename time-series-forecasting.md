# Lag Features for Time-Series Forecasting

## What is a Lag Feature?

A lag feature shifts the target column backwards in time to create a new input column. It gives the model "memory" of the past.

**Example:** To predict today's ice cream sales, the most useful information is:
- What were sales *yesterday*? → `lag_1`
- What were sales *last week*? → `lag_7`

Without lag features, the model has no idea what happened before. With them, it can learn patterns like: *"last week sales were high AND it's a weekend → predict high sales today."*

---

## How `shift()` Works

`shift(n)` moves the entire column **down by `n` rows**, so each row now holds the value from `n` days ago.

```python
import pandas as pd

df = pd.read_csv('sales.csv')
df['date'] = pd.to_datetime(df['date'])
df = df.set_index('date').sort_index()

# Create lag features
df['lag_1'] = df['sales'].shift(1)   # yesterday
df['lag_7'] = df['sales'].shift(7)   # last week
```

### Visual example

| Date  | sales | lag_1 | lag_7 |
|-------|-------|-------|-------|
| Jan 1 | 120   | NaN   | NaN   |
| Jan 2 | 135   | 120   | NaN   |
| Jan 3 | 128   | 135   | NaN   |
| Jan 4 | 142   | 128   | NaN   |
| Jan 8 | 190   | 155   | 120   |
| Jan 9 | 185   | 190   | 135   |

> **Why NaN?** `lag_1` on Jan 1 has no "yesterday", so it's NaN. `lag_7` on Jan 1–7 has no data from a week ago. These rows must be dropped before training — this is normal and expected.

---

## Three Types of Lag Features

### 1. Simple Lag

One specific point in the past. The most direct — *"what happened N days ago?"*

```python
df['lag_1']  = df['sales'].shift(1)   # yesterday
df['lag_7']  = df['sales'].shift(7)   # last week
df['lag_30'] = df['sales'].shift(30)  # last month
```

**Use when:** a specific past day has a direct relationship to today (e.g. same day last week for weekly seasonal data).

---

### 2. Rolling Window

Average (or std, min, max) over the last N days. Smooths out noise to show the recent trend.

```python
w = df['sales'].shift(1)                 # always shift(1) FIRST

df['rolling_7_mean'] = w.rolling(7).mean()
df['rolling_7_std']  = w.rolling(7).std()
df['rolling_7_max']  = w.rolling(7).max()
df['rolling_28_mean']= w.rolling(28).mean()
```

**Use when:** you want *"how has the recent average been?"* rather than a single noisy day.

---

### 3. Expanding Window

Uses **all history** up to that row. Captures the long-run baseline.

```python
df['expanding_mean'] = (
    df['sales']
    .shift(1)
    .expanding()
    .mean()
)
```

**Use when:** you want a historical baseline the model can compare each day against.

---

## The Golden Rule: Always `shift(1)` Before `rolling()`

```python
# WRONG — leaks today's value into the average (data leakage!)
df['rolling_7'] = df['sales'].rolling(7).mean()

# CORRECT — shift(1) first so today is excluded
df['rolling_7'] = df['sales'].shift(1).rolling(7).mean()
```

When the model is predicting today's sales, it must **not** see today's value anywhere in its inputs — not directly, and not hidden inside an average.

---

## How to Choose Which Lags to Use

### Step 1 — Use domain logic

Ask yourself: *what past periods are naturally related to today?*

| Data type        | Recommended lags                              |
|------------------|-----------------------------------------------|
| Daily sales      | lag_1, lag_7, lag_14, lag_365                 |
| Hourly electricity | lag_1, lag_24 (yesterday), lag_168 (last week) |
| Website traffic  | lag_1, lag_7 (weekday patterns repeat weekly) |

### Step 2 — Use autocorrelation (ACF plot)

An ACF plot tells you *statistically* which past values correlate with today's value.

```python
from statsmodels.graphics.tsaplots import plot_acf
import matplotlib.pyplot as plt

plot_acf(df['sales'], lags=30)
plt.title('Which past days correlate with today?')
plt.show()
```

- **Spikes at lag 7, 14, 21** → weekly seasonality exists, include those lags
- **Slowly decaying spikes** → trend exists, include a rolling average

---

## Safe Starter Set for Daily Data

```python
# Simple lags
for lag in [1, 2, 3, 7, 14, 28]:
    df[f'lag_{lag}'] = df['sales'].shift(lag)

# Rolling statistics (always shift(1) first!)
df['rolling_7_mean']  = df['sales'].shift(1).rolling(7).mean()
df['rolling_28_mean'] = df['sales'].shift(1).rolling(28).mean()
df['rolling_7_std']   = df['sales'].shift(1).rolling(7).std()

# Drop rows that still have NaN after lag creation
df = df.dropna()
```

---

## Complete Example: Lag Features with LightGBM

```python
import pandas as pd
import lightgbm as lgb
from sklearn.metrics import mean_absolute_error

# --- Load and index ---
df = pd.read_csv('sales.csv')
df['date'] = pd.to_datetime(df['date'])
df = df.set_index('date').sort_index()

# --- Create lag features ---
for lag in [1, 7, 14]:
    df[f'lag_{lag}'] = df['sales'].shift(lag)

df['rolling_7']  = df['sales'].shift(1).rolling(7).mean()
df['rolling_28'] = df['sales'].shift(1).rolling(28).mean()

# --- Date features ---
df['month']      = df.index.month
df['dayofweek']  = df.index.dayofweek
df['is_weekend'] = (df.index.dayofweek >= 5).astype(int)

# --- Drop NaN rows ---
df = df.dropna()

# --- Chronological train/test split (never shuffle!) ---
split = int(len(df) * 0.8)
features = ['lag_1', 'lag_7', 'lag_14', 'rolling_7', 'rolling_28',
            'month', 'dayofweek', 'is_weekend']

X_train = df[features].iloc[:split]
X_test  = df[features].iloc[split:]
y_train = df['sales'].iloc[:split]
y_test  = df['sales'].iloc[split:]

# --- Train model ---
model = lgb.LGBMRegressor(n_estimators=200, learning_rate=0.05)
model.fit(X_train, y_train)

# --- Evaluate ---
preds = model.predict(X_test)
mae   = mean_absolute_error(y_test, preds)
print(f'MAE: {mae:.2f}')
```

---

## Quick Reference Summary

| Feature type     | Code                                         | What it captures              |
|------------------|----------------------------------------------|-------------------------------|
| Simple lag       | `df['sales'].shift(n)`                       | Value from exactly N days ago |
| Rolling mean     | `df['sales'].shift(1).rolling(n).mean()`     | Average over last N days      |
| Rolling std      | `df['sales'].shift(1).rolling(n).std()`      | Volatility over last N days   |
| Rolling max      | `df['sales'].shift(1).rolling(n).max()`      | Peak over last N days         |
| Expanding mean   | `df['sales'].shift(1).expanding().mean()`    | All-time average up to today  |

> **Key rule:** Always `shift(1)` before `rolling()` to avoid data leakage. Always split data chronologically — never randomly.

# Time-Series Feature Types Guide

---

## 1. Lag Features

### What is it?
A lag feature copies the value of the target column from N steps in the past into a new column.
`lag_1` = yesterday's value, `lag_7` = last week's value, and so on.

```python
df['lag_1']  = df['sales'].shift(1)   # yesterday
df['lag_7']  = df['sales'].shift(7)   # last week
df['lag_14'] = df['sales'].shift(14)  # 2 weeks ago
df['lag_30'] = df['sales'].shift(30)  # last month
```

### Why is it important?
Most time-series data has **autocorrelation** — meaning today's value is closely related to recent past values. Without lag features, the model has zero memory of what happened before. Lag features are the #1 most impactful feature type in time-series forecasting.

> Example: If sales were high yesterday, they are likely high today too. `lag_1` lets the model learn this pattern.

---

## 2. Rolling Window Features

### What is it?
A rolling feature computes a statistic (mean, std, min, max) over a sliding window of the last N days. Instead of one noisy past value, it gives a smoothed summary of recent history.

```python
w = df['sales'].shift(1)              # always shift(1) FIRST to avoid leakage

df['rolling_7_mean']  = w.rolling(7).mean()    # avg of last 7 days
df['rolling_7_std']   = w.rolling(7).std()     # volatility of last 7 days
df['rolling_7_max']   = w.rolling(7).max()     # peak of last 7 days
df['rolling_28_mean'] = w.rolling(28).mean()   # avg of last 28 days
```

### Why is it important?
Single lag values are noisy — one unusually bad day can mislead the model. Rolling features smooth out that noise and capture the **recent trend**. They also help the model understand whether the current moment is above or below the recent average.

> Example: `rolling_7_mean` of 500 on a day with `lag_1` of 300 tells the model: "yesterday was unusually low, the real trend is higher."

---

## 3. Expanding Window Features

### What is it?
An expanding window grows with each row — it computes a statistic over **all past data** up to that point, not just a fixed window.

```python
df['expanding_mean'] = df['sales'].shift(1).expanding().mean()
df['expanding_std']  = df['sales'].shift(1).expanding().std()
```

### Why is it important?
It gives the model a **long-run historical baseline** to compare against. While rolling features capture short-term context, expanding features answer: *"how does today compare to the entire history?"* Useful for detecting long-term growth or seasonal deviation from the norm.

> Example: If `expanding_mean` is 200 but today's sales are 600, the model knows this is an exceptional day — likely a holiday or event.

---

## 4. Date & Time Features

### What is it?
Numeric features extracted directly from the timestamp — month, day of week, quarter, hour, whether it is a holiday, etc.

```python
df['month']      = df.index.month           # 1–12
df['dayofweek']  = df.index.dayofweek       # 0=Monday, 6=Sunday
df['quarter']    = df.index.quarter         # 1–4
df['is_weekend'] = (df.index.dayofweek >= 5).astype(int)
df['week']       = df.index.isocalendar().week.astype(int)
```

### Why is it important?
ML models cannot read dates — they only understand numbers. Date features encode **seasonality and cyclical patterns** into a format the model can learn from. Without them, the model cannot know that Sundays or December are different from other days.

> Example: `is_weekend = 1` combined with `lag_7` showing high sales last Sunday teaches the model that weekends are consistently high.

---

## 5. Difference Features

### What is it?
A difference feature captures **how much the value changed** from one period to the next, rather than the absolute value.

```python
df['diff_1']  = df['sales'].diff(1)    # change vs yesterday
df['diff_7']  = df['sales'].diff(7)    # change vs last week
df['pct_1']   = df['sales'].pct_change(1)   # % change vs yesterday
```

### Why is it important?
Raw sales values can be very different across time (e.g. growing from 100 to 10,000 over years). Difference features focus the model on **momentum and change rate** instead of absolute level. They also help make non-stationary data (data with a trend) more stable and easier to learn from.

> Example: `diff_7 = +50` means sales are 50 units higher than last week — the model can learn if this upward momentum continues.

---

## 6. Target Encoding / Categorical Features

### What is it?
If you have categorical data (store ID, product category, region), you encode each category as a number or as its historical average target value.

```python
# Label encoding
df['store_id'] = df['store_id'].astype('category').cat.codes

# Target mean encoding (average sales per store)
store_means = df.groupby('store_id')['sales'].mean()
df['store_mean_sales'] = df['store_id'].map(store_means)
```

### Why is it important?
Models cannot process text or category labels directly. Encoding them — especially with target means — gives the model **context about each group's baseline behaviour**, which is far more informative than a plain number code.

> Example: Store A has a historical average of 800 sales/day while Store B has 200. Encoding this tells the model to expect different baselines without needing separate models.

---

## Quick Reference

| Feature type       | What it captures                        | Key code                                   |
|--------------------|-----------------------------------------|--------------------------------------------|
| Lag                | Exact value from N periods ago          | `df['sales'].shift(n)`                     |
| Rolling mean       | Average over last N periods             | `df['sales'].shift(1).rolling(n).mean()`   |
| Rolling std        | Volatility over last N periods          | `df['sales'].shift(1).rolling(n).std()`    |
| Expanding mean     | All-time average up to today            | `df['sales'].shift(1).expanding().mean()`  |
| Date features      | Seasonality and calendar patterns       | `df.index.month`, `df.index.dayofweek`     |
| Difference         | Change / momentum from past period      | `df['sales'].diff(n)`                      |
| Target encoding    | Group-level historical baseline         | `df.groupby('cat')['sales'].mean()`        |

---

## Golden Rules

1. **Always `shift(1)` before `rolling()`** — prevents today's value leaking into its own features.
2. **Drop NaN rows after feature creation** — lag features leave NaN at the start of the dataframe.
3. **Split chronologically, never randomly** — train on past, test on future.
4. **Fit the scaler only on training data** — `fit_transform` on train, `transform` only on test.
