# Bias vs Variance + Overfitting / Underfitting + Regularization

## Goal of Machine Learning

The main goal of Machine Learning is:

> Learn patterns from training data and perform well on unseen data.

This ability is called:

**Generalization**

Example:

Training:

```text
House Size  → House Price

1000 sqft   → 50 lakh
1500 sqft   → 75 lakh
2000 sqft   → 1 crore
```

New data:

```text
1800 sqft → ?
```

A good model should predict correctly.

---

# 1. Bias

## Definition

Bias means:

> Error caused because the model is too simple and cannot capture the real pattern.

The model makes wrong assumptions.

---

## High Bias

High Bias means:

- Model is too simple
- Model cannot learn patterns properly


Example:

Real pattern:

```text
       *
    *
 *
*
```

Model learns:

```text
----------------
Straight Line
```

The model cannot understand complexity.

---

## Causes of High Bias

- Simple algorithm
- Not enough features
- Wrong assumptions
- Training time too short
- Too much regularization

---

## Result of High Bias

High Bias causes:

# Underfitting

---

# 2. Underfitting

## Meaning

Underfitting happens when:

> Model fails to learn patterns from training data.

---

## Example

Dog vs Cat classifier:

Model learns:

```text
Animal has ears = Cat
```

Problem:

Dogs also have ears.

Model learned too simple rule.

---

## Symptoms

Training Accuracy:

```text
Low
Example: 60%
```

Testing Accuracy:

```text
Low
Example: 58%
```

Both training and testing performance are bad.

---

## How To Fix Underfitting

### 1. Increase Model Complexity

Replace:

```text
Linear Regression
```

With:

```text
Random Forest

XGBoost

Neural Network
```

---

### 2. Add More Features

Bad features:

```text
weight
```

Better features:

```text
height

shape

color

texture
```

---

### 3. Train Longer

Increase:

```text
Epochs

Training time
```

---

### 4. Reduce Regularization

Too much regularization makes model too simple.

---

# 3. Variance

## Definition

Variance means:

> Error because the model is too sensitive to training data.

The model memorizes instead of learning.

---

## High Variance

High variance means:

- Model too complex
- Learns noise
- Memorizes data


This causes:

# Overfitting

---

# 4. Overfitting

## Meaning

Overfitting happens when:

> Model performs excellent on training data but poorly on new data.

---

## Example

Dog classifier:

Training images:

All dogs are on grass.

Model learns:

```text
Grass = Dog
```

Instead of learning dog features.

New dog image without grass:

Wrong prediction.

---

## Symptoms

Training Accuracy:

```text
99%
```

Testing Accuracy:

```text
70%
```

Large gap = Overfitting

---

# How To Fix Overfitting

## 1. Add More Training Data

Before:

```text
1000 images
```

After:

```text
1 million images
```

More data improves generalization.

---

## 2. Data Augmentation

Create variations:

```text
Rotate image

Crop image

Change brightness

Flip image
```

---

## 3. Reduce Model Complexity

Example:

Reduce:

```text
Layers

Parameters

Tree depth
```

---

## 4. Feature Selection

Keep useful features.

Example:

House price prediction:

Good:

```text
Area

Location

Rooms
```

Remove:

```text
Owner name

House color
```

---

# 5. Regularization

## Definition

Regularization is:

> A technique used to reduce overfitting by controlling model complexity.

Purpose:

```text
Prevent memorization

Improve generalization
```

---

# Types of Regularization

---

# L1 Regularization (Lasso)

## Purpose

Feature selection.

It can remove unnecessary features.

---

Example:

Features:

```text
Experience ✔

Education ✔

Favorite color ❌
```

L1 can make useless feature weight:

```text
0
```

---

Use when:

- Many features
- Need automatic feature selection


---

# L2 Regularization (Ridge)

## Purpose

Controls very large weights.

Keeps all features.

---

Before:

```text
Feature weights:

Income = 100000

Age = 500
```

After L2:

```text
Income = 1000

Age = 50
```

Model becomes stable.

---

Use when:

- Reduce overfitting
- Keep all features

---

# Dropout

Used in:

Deep Learning / Neural Networks

---

How it works:

During training randomly disable neurons.

Before:

```text
Neuron 1 ON

Neuron 2 ON

Neuron 3 ON
```

During dropout:

```text
Neuron 1 ON

Neuron 2 OFF

Neuron 3 ON
```

Benefit:

Model does not depend on one path.

---

# Early Stopping

Stops training before overfitting.

Example:

```text
Epoch 1

Accuracy improving


Epoch 20

Best validation score


Epoch 50

Training accuracy increases

Validation decreases
```

Stop at best validation performance.

---

# Bias Variance Tradeoff

Goal:

Balance both.

---

## High Bias

```text
Model too simple

↓

Underfitting
```

---

## High Variance

```text
Model too complex

↓

Overfitting
```

---

## Ideal Model

```text
Low Bias

+

Low Variance

=

Good Generalization
```

---

# Exam Examples

## Example 1

Model Result:

Training Accuracy:

```text
99%
```

Validation Accuracy:

```text
65%
```

Problem:

```text
Overfitting
```

Solution:

- Add regularization
- Add more data
- Reduce complexity


---

## Example 2

Model Result:

Training Accuracy:

```text
55%
```

Validation Accuracy:

```text
54%
```

Problem:

```text
Underfitting
```

Solution:

- Increase complexity
- Add features
- Train longer

---

# Google Exam Cheat Sheet

| Situation | Problem | Solution |
|---|---|---|
| Train bad + Test bad | Underfitting | Increase complexity |
| Train good + Test bad | Overfitting | Regularization |
| High Bias | Model too simple | Add features/model |
| High Variance | Model too complex | Reduce complexity |
| Too many features | Overfit risk | L1 |
| Large weights | Overfit risk | L2 |
| Neural Network overfits | Overfitting | Dropout |
| Training too long | Overfitting | Early stopping |

---

# Simple Memory Trick

Bias:

```text
Model learned too little
```

Variance:

```text
Model learned too much
```

Underfitting:

```text
Need more learning power
```

Overfitting:

```text
Need more control
```
