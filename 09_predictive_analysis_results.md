# Predictive Analysis — Classification Results

Author: Vishvi

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.model_selection import train_test_split, GridSearchCV
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.svm import SVC
from sklearn.tree import DecisionTreeClassifier
from sklearn.neighbors import KNeighborsClassifier
from sklearn.metrics import confusion_matrix, classification_report, jaccard_score

# Load dataset
url = 'https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DS0321EN-SkillsNetwork/datasets/dataset_part_2.csv'
df  = pd.read_csv(url)

# Feature engineering
features = pd.get_dummies(df[[
    'FlightNumber','PayloadMass','Flights','Block',
    'ReusedCount','Orbit','LaunchSite','LandingPad',
    'Serial','GridFins','Reused','Legs'
]])

X = features
y = df['Class']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=2)
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test  = scaler.transform(X_test)

results = {}

# Model 1: Logistic Regression
lr_params = {'C':[0.01,0.1,1,10], 'max_iter':[200]}
lr = GridSearchCV(LogisticRegression(), lr_params, cv=10)
lr.fit(X_train, y_train)
results['Logistic Regression'] = lr.score(X_test, y_test)
print('LR Accuracy:', results['Logistic Regression'])

# Model 2: SVM
svm_params = {'C':[0.1,1,10], 'kernel':['rbf','linear']}
svm = GridSearchCV(SVC(), svm_params, cv=10)
svm.fit(X_train, y_train)
results['SVM'] = svm.score(X_test, y_test)
print('SVM Accuracy:', results['SVM'])

# Model 3: Decision Tree
dt_params = {'max_depth':[2,4,6,8,10], 'min_samples_split':[2,5,10]}
dt = GridSearchCV(DecisionTreeClassifier(), dt_params, cv=10)
dt.fit(X_train, y_train)
results['Decision Tree'] = dt.score(X_test, y_test)
print('DT Accuracy:', results['Decision Tree'])

# Model 4: KNN
knn_params = {'n_neighbors':[2,4,6,8,10]}
knn = GridSearchCV(KNeighborsClassifier(), knn_params, cv=10)
knn.fit(X_train, y_train)
results['KNN'] = knn.score(X_test, y_test)
print('KNN Accuracy:', results['KNN'])

# Plot results comparison
plt.figure(figsize=(10,5))
plt.bar(results.keys(), results.values(), color=['#5b8fff','#00e5a0','#ff6b6b','#ffd166'])
plt.title('Model Accuracy Comparison', fontsize=14)
plt.ylabel('Accuracy')
plt.ylim(0, 1)
for i, (k,v) in enumerate(results.items()):
    plt.text(i, v+0.01, str(round(v,3)), ha='center', fontweight='bold')
plt.tight_layout()
plt.savefig('plots/07_model_comparison.png')
plt.show()

# Best model confusion matrix
best_model_name = max(results, key=results.get)
best_model = {'Logistic Regression':lr,'SVM':svm,'Decision Tree':dt,'KNN':knn}[best_model_name]
y_pred = best_model.predict(X_test)

cm = confusion_matrix(y_test, y_pred)
plt.figure(figsize=(7,5))
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues',
            xticklabels=['Failure','Success'],
            yticklabels=['Failure','Success'])
plt.title('Confusion Matrix - ' + best_model_name, fontsize=14)
plt.ylabel('Actual')
plt.xlabel('Predicted')
plt.tight_layout()
plt.savefig('plots/08_confusion_matrix.png')
plt.show()

print('\nBest model:', best_model_name)
print('Best accuracy:', round(results[best_model_name], 3))
print(classification_report(y_test, y_pred, target_names=['Failure','Success']))
```

## Classification Results Summary

| Model | Accuracy | Best Parameters |
|---|---|---|
| Logistic Regression | 0.833 | C=0.01 |
| SVM | 0.833 | C=1, kernel=linear |
| Decision Tree | 0.722 | max_depth=6 |
| KNN | 0.833 | n_neighbors=10 |

All three top models tie at 83.3% accuracy.
SVM with linear kernel selected as final model for
its better generalisation and interpretability.
