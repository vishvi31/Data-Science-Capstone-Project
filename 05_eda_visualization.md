# EDA with Visualization — SpaceX Falcon 9

Author: Vishvi

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Load dataset
url = 'https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DS0321EN-SkillsNetwork/datasets/dataset_part_2.csv'
df = pd.read_csv(url)
print(df.shape)
print(df.head())

# --- Plot 1: Launch success count ---
plt.figure(figsize=(8,5))
sns.countplot(data=df, x='Class', palette=['#ff6b6b','#00e5a0'])
plt.title('Falcon 9 First Stage Landing Outcomes', fontsize=14)
plt.xticks([0,1], ['Failed (0)', 'Success (1)'])
plt.xlabel('Landing Outcome')
plt.ylabel('Count')
plt.tight_layout()
plt.savefig('plots/01_landing_outcomes.png')
plt.show()

# --- Plot 2: Success rate by orbit ---
orbit_success = df.groupby('Orbit')['Class'].mean().sort_values(ascending=False)
plt.figure(figsize=(12,5))
orbit_success.plot(kind='bar', color='steelblue', edgecolor='black')
plt.title('Success Rate by Orbit Type', fontsize=14)
plt.xlabel('Orbit')
plt.ylabel('Success Rate')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.savefig('plots/02_success_by_orbit.png')
plt.show()

# --- Plot 3: Success rate by launch site ---
site_success = df.groupby('LaunchSite')['Class'].mean().sort_values(ascending=False)
plt.figure(figsize=(10,5))
site_success.plot(kind='bar', color='coral', edgecolor='black')
plt.title('Success Rate by Launch Site', fontsize=14)
plt.xlabel('Launch Site')
plt.ylabel('Success Rate')
plt.xticks(rotation=0)
plt.tight_layout()
plt.savefig('plots/03_success_by_site.png')
plt.show()

# --- Plot 4: Payload mass vs success ---
plt.figure(figsize=(10,5))
sns.scatterplot(data=df, x='PayloadMass', y='Class',
                hue='LaunchSite', s=80, palette='Set2')
plt.title('Payload Mass vs Landing Success', fontsize=14)
plt.xlabel('Payload Mass (kg)')
plt.ylabel('Landing Success (1=Yes)')
plt.tight_layout()
plt.savefig('plots/04_payload_vs_success.png')
plt.show()

# --- Plot 5: Yearly success trend ---
df['Year'] = pd.to_datetime(df['Date']).dt.year
yearly = df.groupby('Year')['Class'].mean()
plt.figure(figsize=(10,5))
plt.plot(yearly.index, yearly.values, marker='o',
         color='#5b8fff', linewidth=2.5)
plt.fill_between(yearly.index, yearly.values, alpha=0.15, color='#5b8fff')
plt.title('Yearly Landing Success Rate Trend', fontsize=14)
plt.xlabel('Year')
plt.ylabel('Success Rate')
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.savefig('plots/05_yearly_trend.png')
plt.show()

# --- Plot 6: Correlation heatmap ---
numeric_cols = df.select_dtypes(include='number').columns
plt.figure(figsize=(12,8))
sns.heatmap(df[numeric_cols].corr(), annot=True, fmt='.2f',
            cmap='coolwarm', center=0)
plt.title('Feature Correlation Heatmap', fontsize=14)
plt.tight_layout()
plt.savefig('plots/06_correlation_heatmap.png')
plt.show()

print('All 6 EDA plots generated!')
```
