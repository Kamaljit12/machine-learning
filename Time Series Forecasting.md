# Time Series Forecasting - Fundamentals

# What is Time Series Data?

A Time Series is a sequence of observations collected over time at regular intervals.

---

# Examples

```text
Daily Sales

Monthly Revenue

Stock Prices

Oil Prices

Weather Data

Electricity Consumption
```

---

# Example Dataset

| Date | Sales |
|--------|--------|
| Jan | 100 |
| Feb | 120 |
| Mar | 130 |
| Apr | 150 |

---

# Difference Between Normal ML Data and Time Series Data

| Traditional ML | Time Series |
|---------------|-------------|
| Order doesn't matter | Order matters |
| Rows independent | Rows dependent |
| Random Train-Test Split | Chronological Split |
| Predict Current Value | Predict Future Value |

---

# Example

Traditional ML:

```text
House Price Prediction
```

Every row is independent.

---

Time Series:

```text
Tomorrow's Sales Prediction
```

Today's sales depend on previous sales.

---

# Core Components of Time Series

Every Time Series usually consists of:

```text
Trend

Seasonality

Cyclic Patterns

Noise
```

---

# Time Series Equation

```text
Time Series

=

Trend

+

Seasonality

+

Cyclic Pattern

+

Noise
```

---

# 1. Trend

# Most Important Component

---

## Definition

Trend represents the long-term direction of a time series.

---

# Types of Trend

### Upward Trend

```text
Sales increase over time.
```

Example:

```text
100

150

200

250

300
```

---

### Downward Trend

```text
Sales decrease over time.
```

Example:

```text
500

450

400

350

300
```

---

### Flat Trend

```text
No significant increase or decrease.
```

Example:

```text
100

102

98

101

99
```

---

# Real Examples

### Upward Trend

```text
E-commerce Sales

Population Growth

Internet Users
```

---

### Downward Trend

```text
DVD Sales

Newspaper Sales
```

---

# Interview Question

### What is Trend?

## Strong Answer

```text
Trend represents the long-term movement or direction of a time series, indicating whether values generally increase, decrease, or remain stable over time.
```

---

# How to Identify Trend?

### Visual Inspection

Plot data.

---

### Moving Average

Smooth fluctuations.

---

### Decomposition Techniques

Separate trend component.

---

# Interview Question

### Why is Trend Important?

## Strong Answer

```text
Trend helps understand long-term behavior and is essential for forecasting future values accurately.
```

---

# 2. Seasonality

# Extremely Important

---

## Definition

Seasonality refers to repeating patterns that occur at fixed and known intervals.

---

# Examples

### Ice Cream Sales

```text
High in Summer

Low in Winter
```

Repeats every year.

---

### Electricity Usage

```text
High during daytime

Low at night
```

Repeats daily.

---

### Retail Sales

```text
High during Diwali

High during Christmas
```

Repeats annually.

---

# Characteristics

```text
Fixed Frequency

Predictable

Repeating Pattern
```

---

# Examples of Seasonal Periods

| Data | Seasonal Period |
|---------|---------|
| Hourly | 24 Hours |
| Daily | 7 Days |
| Monthly | 12 Months |
| Quarterly | 4 Quarters |

---

# Interview Question

### What is Seasonality?

## Strong Answer

```text
Seasonality refers to recurring patterns that repeat at fixed and known intervals due to calendar-related or periodic factors.
```

---

# Trend vs Seasonality

# Most Asked Question

| Trend | Seasonality |
|---------|---------|
| Long-Term Movement | Repeating Pattern |
| May Not Repeat | Always Repeats |
| Up/Down Direction | Cyclic Within Fixed Period |

---

# Example

Sales:

```text
Increase every year
```

Trend.

---

Sales:

```text
Increase every December
```

Seasonality.

---

# Interview Answer

```text
Trend describes the overall long-term direction, whereas seasonality represents repeating patterns occurring at regular intervals.
```

---

# 3. Cyclic Patterns

# Interview Favourite

---

## Definition

Cyclic Patterns are fluctuations that occur over long periods but do not have a fixed frequency.

---

# Difference from Seasonality

Seasonality:

```text
Fixed Frequency
```

---

Cycles:

```text
No Fixed Frequency
```

---

# Example

Economic Growth

```text
Boom

↓

Recession

↓

Recovery

↓

Boom
```

---

Duration:

```text
2 Years

5 Years

8 Years
```

Not fixed.

---

# Real Examples

### Stock Market Cycles

### Economic Cycles

### Housing Market Cycles

### Oil Price Cycles

---

# Interview Question

### What is a Cyclic Pattern?

## Strong Answer

```text
Cyclic patterns are long-term fluctuations that occur over irregular periods and do not have a fixed or predictable frequency.
```

---

# Seasonality vs Cyclic Patterns

# Extremely Important

| Seasonality | Cyclic Pattern |
|-------------|-------------|
| Fixed Frequency | Variable Frequency |
| Predictable | Less Predictable |
| Calendar Based | Economic/Event Driven |
| Annual Festival Sales | Business Cycles |

---

# Example

Sales rise every December:

```text
Seasonality
```

---

Sales rise every 4-7 years:

```text
Cyclic Pattern
```

---

# Interview Question

### Difference Between Seasonality and Cyclic Patterns?

## Strong Answer

```text
Seasonality occurs at fixed and predictable intervals, while cyclic patterns occur over irregular periods and are often influenced by economic or business conditions.
```

---

# 4. Noise

# Very Important

---

## Definition

Noise represents random fluctuations that cannot be explained by trend, seasonality, or cyclic patterns.

---

# Examples

Unexpected Events:

```text
COVID-19

Natural Disaster

Political Event

Server Failure
```

---

# Example

Expected Sales:

```text
100
```

Actual Sales:

```text
180
```

Due to:

```text
Unexpected Viral Marketing
```

This may be noise.

---

# Characteristics

```text
Random

Unpredictable

Irregular
```

---

# Interview Question

### What is Noise?

## Strong Answer

```text
Noise represents random and unpredictable variations in a time series that cannot be explained by trend, seasonality, or cyclic behavior.
```

---

# Why is Noise Important?

Too much noise:

```text
Poor Forecast Accuracy

Overfitting Risk

Difficult Pattern Detection
```

---

# Goal in Forecasting

```text
Capture Trend

Capture Seasonality

Capture Cycles

Ignore Noise
```

---

# Complete Example

Monthly Sales

```text
Trend:
Sales increasing every year.
```

---

```text
Seasonality:
December sales spike annually.
```

---

```text
Cyclic:
Economic boom increases sales for several years.
```

---

```text
Noise:
Unexpected COVID event.
```

---

# Most Asked Interview Questions

## Beginner

```text
1. What is Time Series Data?
2. What is Trend?
3. What is Seasonality?
4. What is Cyclic Pattern?
5. What is Noise?
```

---

## Intermediate

```text
1. Trend vs Seasonality?
2. Seasonality vs Cyclic Pattern?
3. Why is Time Series different from ML?
4. How do you identify trend?
5. Why is noise problematic?
```

---

## Advanced

```text
1. Time Series Components?
2. How does seasonality affect forecasting?
3. How do forecasting models handle trend?
4. How do you remove noise?
5. Why must temporal order be preserved?
```

---

# 🚨 Top 5 Questions Asked in Almost Every Interview

## 1. What is Trend?

### Strong Answer

```text
Trend is the long-term upward, downward, or stable movement of a time series.
```

---

## 2. What is Seasonality?

### Strong Answer

```text
Seasonality is a repeating pattern occurring at fixed and predictable intervals.
```

---

## 3. Trend vs Seasonality?

### Strong Answer

```text
Trend is the overall direction of the series, while seasonality represents recurring patterns at fixed intervals.
```

---

## 4. Seasonality vs Cyclic Pattern?

### Strong Answer

```text
Seasonality has a fixed frequency, while cyclic patterns occur over irregular periods.
```

---

## 5. What is Noise?

### Strong Answer

```text
Noise represents random and unpredictable fluctuations that cannot be explained by trend, seasonality, or cyclic patterns.
```

---

# Interviewer's Favourite Scenario Question

### Question

```text
Sales increase every December.

Is this:

Trend

Seasonality

or Cyclic Pattern?
```

### Strong Answer

```text
Seasonality.

Because the increase occurs repeatedly at a fixed interval every year.
```

---

# One-Liner That Impresses Interviewers

```text
A time series is typically composed of trend, seasonality, cyclic patterns, and noise, and successful forecasting models aim to capture the first three while minimizing the impact of noise.
```

