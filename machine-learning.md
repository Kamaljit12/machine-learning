# Machine Learning (ML) Notes

# 1. What is Machine Learning?

Machine Learning (ML) is a subset of Artificial Intelligence (AI) that enables computers to learn patterns from data and make predictions or decisions without being explicitly programmed.

### Traditional Programming
Input + Rules → Output

### Machine Learning
Input + Output Data → Learn Rules (Model)

After learning:

New Input → Model → Prediction

---

# 2. Why Machine Learning?

In many real-world problems, writing rules manually is difficult.

### Examples

#### Spam Detection
Instead of writing thousands of spam rules, ML learns patterns from previous emails.

#### House Price Prediction
ML learns the relationship between:
- Area
- Location
- Number of rooms
- Amenities

and predicts house prices.

#### Recommendation Systems
Used by:
- Netflix
- YouTube
- Amazon
- Spotify

to recommend content based on user behavior.

---

# 3. Artificial Intelligence vs Machine Learning vs Deep Learning

AI
│
├── Machine Learning
│   │
│   └── Deep Learning

## Artificial Intelligence (AI)

Making machines perform tasks that normally require human intelligence.

Examples:
- Chatbots
- Self-driving cars
- Voice assistants

## Machine Learning (ML)

Machines learn from data.

Examples:
- Fraud detection
- Recommendation systems
- Price prediction

## Deep Learning (DL)

Subset of ML that uses Neural Networks with many layers.

Examples:
- Image recognition
- Speech recognition
- ChatGPT
- Autonomous vehicles

---

# 4. Types of Machine Learning

There are mainly 4 types:

1. Supervised Learning
2. Unsupervised Learning
3. Semi-Supervised Learning
4. Reinforcement Learning

---

# 5. Supervised Learning

## Definition

Model learns using labeled data.

Labeled Data:
Input + Correct Output

Example:

| Area | Price |
|--------|--------|
| 1000 | 50L |
| 1500 | 75L |
| 2000 | 100L |

The model learns the relationship between area and price.

### Goal

Predict output for unseen data.

---

## Types of Supervised Learning

### A. Regression

Predict Continuous Values.

Examples:
- House Price Prediction
- Sales Forecasting
- Temperature Prediction

Output:

10.5
25.6
100.3

### B. Classification

Predict Categories.

Examples:
- Spam/Not Spam
- Fraud/Not Fraud
- Disease/No Disease

Output:

Yes / No

Spam / Not Spam

---

# 6. Unsupervised Learning

## Definition

Model learns patterns from unlabeled data.

Input Only

No target column available.

### Example

Customer Segmentation

Data:

| Age | Income |
|------|---------|
| 25 | 30K |
| 27 | 35K |
| 55 | 90K |

ML automatically groups similar customers.

---

## Main Tasks

### Clustering

Group similar data points.

Algorithms:
- K-Means
- DBSCAN
- Hierarchical Clustering

Example:
Customer Segmentation

---

### Dimensionality Reduction

Reduce number of features.

Algorithms:
- PCA
- t-SNE
- UMAP

Benefits:
- Faster training
- Better visualization
- Less storage

---

# 7. Semi-Supervised Learning

Combination of:

- Small labeled data
- Large unlabeled data

Example:

Medical Imaging

Labeling X-rays is expensive.

Use:
- 1000 labeled images
- 100000 unlabeled images

Model learns from both.

---

# 8. Reinforcement Learning

## Definition

Agent learns by interacting with environment.

### Components

Agent → Learner

Environment → World

Action → What agent does

Reward → Feedback

State → Current situation

---

### Example

Self-driving Car

Action:
- Accelerate
- Brake
- Turn

Reward:
+10 Safe driving

Penalty:
-10 Collision

Goal:
Maximize total reward.

---

# 9. Machine Learning Workflow

Data Collection
↓
Data Cleaning
↓
Exploratory Data Analysis (EDA)
↓
Feature Engineering
↓
Train-Test Split
↓
Model Training
↓
Model Evaluation
↓
Hyperparameter Tuning
↓
Deployment
↓
Monitoring

---

# 10. Dataset Structure

Example:

| Age | Salary | Purchased |
|------|---------|-----------|
| 25 | 30000 | No |
| 35 | 60000 | Yes |

### Features (X)

Input Columns

- Age
- Salary

### Target (Y)

Output Column

- Purchased

---

# 11. Training, Validation and Testing

## Training Set

Used to learn patterns.

Usually:
70-80%

---

## Validation Set

Used for tuning model.

Usually:
10-15%

---

## Test Set

Used for final evaluation.

Usually:
10-20%

---

# 12. Common ML Algorithms

## Regression

- Linear Regression
- Ridge Regression
- Lasso Regression
- ElasticNet

---

## Classification

- Logistic Regression
- Decision Tree
- Random Forest
- XGBoost
- LightGBM
- CatBoost
- SVM
- KNN
- Naive Bayes

---

## Clustering

- K-Means
- DBSCAN
- Hierarchical Clustering

---

## Deep Learning

- ANN
- CNN
- RNN
- LSTM
- Transformer

---

# 13. Overfitting and Underfitting

## Underfitting

Model learns too little.

Training Accuracy → Low

Testing Accuracy → Low

Example:
Trying to fit a straight line on complex data.

---

## Overfitting

Model memorizes training data.

Training Accuracy → Very High

Testing Accuracy → Low

Example:
Student memorizes answers instead of understanding concepts.

---

## Good Fit

Training Accuracy ≈ Testing Accuracy

Generalizes well.

---

# 14. Bias-Variance Tradeoff

## High Bias

Model too simple.

Result:
Underfitting

Examples:
- Very shallow tree
- Simple linear model

---

## High Variance

Model too complex.

Result:
Overfitting

Examples:
- Very deep tree
- Large neural network

---

Goal:

Low Bias + Low Variance

---

# 15. Evaluation Metrics

## Regression Metrics

### MAE

Mean Absolute Error

Average absolute difference.

### MSE

Mean Squared Error

Squares errors.

### RMSE

Root Mean Squared Error

Most popular.

### R² Score

Measures goodness of fit.

Range:

0 to 1

Higher is better.

---

## Classification Metrics

### Accuracy

Correct Predictions / Total Predictions

---

### Precision

Among predicted positives:

How many are actually positive?

---

### Recall

Among actual positives:

How many identified correctly?

---

### F1 Score

Balance of:

- Precision
- Recall

---

### ROC-AUC

Measures classification performance.

Higher is better.

---

# 16. Feature Engineering

Creating better features from raw data.

Examples:

Date:
2025-05-10

Create:
- Day
- Month
- Year
- Weekday

Benefits:
- Better model performance
- Better insights

---

# 17. Feature Scaling

Makes features comparable.

Example:

Age = 25

Salary = 100000

Different scales may affect algorithms.

---

## Standardization

Mean = 0

Standard Deviation = 1

Used in:
- Logistic Regression
- SVM
- KNN

---

## Normalization

Scale between:

0 and 1

Used in:
- Neural Networks

---

# 18. Cross Validation

Instead of one train-test split:

Train multiple times using different splits.

Most common:

K-Fold Cross Validation

Benefits:
- More reliable evaluation
- Better generalization estimate

---

# 19. Hyperparameters

Settings chosen before training.

Examples:

Random Forest:
- n_estimators
- max_depth

XGBoost:
- learning_rate
- max_depth

Neural Networks:
- epochs
- batch_size
- learning_rate

---

# 20. Model Deployment

After training:

Deploy model to production.

Tools:
- FastAPI
- Flask
- Docker
- Kubernetes
- AWS
- Azure
- GCP

---

# 21. Real-World Machine Learning Applications

### Finance
- Fraud Detection
- Credit Scoring
- Stock Prediction

### Healthcare
- Disease Prediction
- Medical Imaging

### Retail
- Demand Forecasting
- Customer Segmentation

### Manufacturing
- Predictive Maintenance

### Oil & Gas
- Demand Forecasting
- Production Optimization
- Price Forecasting

### AI Applications
- ChatGPT
- Copilot
- Claude
- Gemini

---

# Interview Revision (Must Remember)

✅ AI > ML > DL

✅ Supervised Learning → Labeled Data

✅ Unsupervised Learning → Unlabeled Data

✅ Regression → Continuous Output

✅ Classification → Categorical Output

✅ Overfitting → High Train, Low Test Performance

✅ Underfitting → Low Train, Low Test Performance

✅ Bias → Simplicity Error

✅ Variance → Complexity Error

✅ Precision → Predicted Positive Accuracy

✅ Recall → Actual Positive Coverage

✅ F1 Score → Balance of Precision and Recall

✅ Feature Engineering often improves performance more than changing algorithms

✅ Data quality is usually more important than model complexity

---

# One-Line Summary

Machine Learning is the process of enabling computers to learn patterns from historical data and use those patterns to make predictions, classifications, recommendations, or decisions on new unseen data.
