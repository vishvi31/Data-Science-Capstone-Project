# Predictive Analysis Methodology

Author: Vishvi

---

## Problem Type

Binary Classification:
- Class 1 = First stage lands successfully
- Class 0 = First stage does not land

---

## Feature Engineering

```python
# One-hot encode categorical features
features = pd.get_dummies(df[[
    'FlightNumber','PayloadMass','Flights','Block',
    'ReusedCount','Orbit','LaunchSite','LandingPad',
    'Serial','GridFins','Reused','Legs'
]])

X = features
y = df['Class']

# Standardise numerical features
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X = scaler.fit_transform(X)
```

---

## Train Test Split

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=2
)
print('Train size:', X_train.shape)
print('Test size:', X_test.shape)
```

---

## Models Evaluated

| Model | Tuning Method |
|---|---|
| Logistic Regression | GridSearchCV |
| Support Vector Machine | GridSearchCV |
| Decision Tree | GridSearchCV |
| K-Nearest Neighbours | GridSearchCV |

---

## Evaluation Metrics

| Metric | Why used |
|---|---|
| Accuracy | Overall correctness |
| F1 Score | Balance of precision and recall |
| Confusion Matrix | Breakdown of TP, FP, TN, FN |
| Jaccard Score | Similarity between predicted and actual |

---

*Built by Vishvi | IBM Data Science Capstone*
