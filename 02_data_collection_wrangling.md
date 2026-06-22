# Data Collection and Data Wrangling Methodology

Author: Vishvi

---

## Data Sources

| Source | Method | Data Collected |
|---|---|---|
| SpaceX REST API | requests.get() | Launch records, payload, outcomes |
| Wikipedia | BeautifulSoup web scraping | Historical Falcon 9 launches |

---

## Data Collection — SpaceX API

```python
import requests
import pandas as pd

spacex_url = 'https://api.spacexdata.com/v4/launches/past'
response   = requests.get(spacex_url)
data       = response.json()

df = pd.json_normalize(data)
print(df.shape)
print(df.columns.tolist())
```

---

## Data Collection — Web Scraping

```python
from bs4 import BeautifulSoup

url = 'https://en.wikipedia.org/wiki/List_of_Falcon_9_and_Falcon_Heavy_launches'
response = requests.get(url)
soup     = BeautifulSoup(response.text, 'html.parser')
tables   = soup.find_all('table', class_='wikitable')
df_wiki  = pd.read_html(str(tables[0]))[0]
print(df_wiki.head())
```

---

## Data Wrangling Steps

```python
# 1. Select relevant columns
cols = ['FlightNumber','Date','BoosterVersion','PayloadMass',
        'Orbit','LaunchSite','Outcome','Flights','GridFins',
        'Reused','Legs','LandingPad','Block','ReusedCount','Serial']
df = df[cols]

# 2. Create landing label (our target variable)
landing_outcomes = {
    'True Ocean':False,'False Ocean':False,
    'True RTLS':True,'False RTLS':False,
    'True ASDS':True,'False ASDS':False,
    'None ASDS':False,'None None':False
}
df['Class'] = df['Outcome'].map(landing_outcomes).astype(int)

# 3. Handle missing values
print(df.isnull().sum())
df['PayloadMass'] = df['PayloadMass'].fillna(df['PayloadMass'].mean())
df['LandingPad']  = df['LandingPad'].fillna('None')

# 4. Check class balance
print(df['Class'].value_counts())
print('Success rate:', df['Class'].mean().round(3))
```

---

## Final Dataset Summary

| Property | Value |
|---|---|
| Total launches | 90 |
| Features | 15 |
| Target variable | Class (1=Success, 0=Failure) |
| Success rate | 67% |
| Missing PayloadMass | Filled with mean |

---

*Built by Vishvi | IBM Data Science Capstone*
