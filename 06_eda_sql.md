# EDA with SQL — SpaceX Falcon 9

Author: Vishvi

```python
import sqlite3
import pandas as pd

# Load dataset into SQLite
url = 'https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DS0321EN-SkillsNetwork/datasets/dataset_part_1.csv'
df  = pd.read_csv(url)

conn = sqlite3.connect(':memory:')
df.to_sql('SPACEXTBL', conn, if_exists='replace', index=False)

def sql(query, title=''):
    print('\n' + '='*55)
    print(' ' + title)
    print('='*55)
    result = pd.read_sql_query(query, conn)
    print(result.to_string(index=False))
    return result

# SQL 1: Display unique launch sites
sql(
    'SELECT DISTINCT Launch_Site FROM SPACEXTBL;',
    'Q1: Unique Launch Sites'
)

# SQL 2: First 5 records where launch site begins with CCA
sql(
    "SELECT * FROM SPACEXTBL WHERE Launch_Site LIKE 'CCA%' LIMIT 5;",
    'Q2: First 5 records from CCA launch sites'
)

# SQL 3: Total payload mass for NASA (CRS) missions
sql(
    "SELECT SUM(PAYLOAD_MASS__KG_) AS Total_Payload FROM SPACEXTBL WHERE Customer = 'NASA (CRS)';",
    'Q3: Total payload mass for NASA CRS'
)

# SQL 4: Average payload mass for booster F9 v1.1
sql(
    "SELECT AVG(PAYLOAD_MASS__KG_) AS Avg_Payload FROM SPACEXTBL WHERE Booster_Version LIKE 'F9 v1.1%';",
    'Q4: Average payload for F9 v1.1'
)

# SQL 5: Date of first successful landing on ground pad
sql(
    "SELECT MIN(Date) AS First_Success FROM SPACEXTBL WHERE Mission_Outcome = 'Success' AND Landing_Outcome = 'Success (ground pad)';",
    'Q5: First successful ground pad landing date'
)

# SQL 6: Booster versions with successful drone ship landings and payload 4000-6000 kg
sql(
    "SELECT DISTINCT Booster_Version FROM SPACEXTBL WHERE Landing_Outcome = 'Success (drone ship)' AND PAYLOAD_MASS__KG_ BETWEEN 4000 AND 6000;",
    'Q6: Boosters with drone ship success and payload 4000-6000 kg'
)

# SQL 7: Total success and failure counts for mission outcomes
sql(
    "SELECT Mission_Outcome, COUNT(*) AS Count FROM SPACEXTBL GROUP BY Mission_Outcome;",
    'Q7: Total success and failure mission counts'
)

# SQL 8: Booster version with max payload mass
sql(
    'SELECT Booster_Version, MAX(PAYLOAD_MASS__KG_) AS Max_Payload FROM SPACEXTBL;',
    'Q8: Booster version with max payload'
)

# SQL 9: 2015 drone ship failures - month, booster, launch site
sql(
    "SELECT SUBSTR(Date,6,2) AS Month, Booster_Version, Launch_Site FROM SPACEXTBL WHERE SUBSTR(Date,1,4)='2015' AND Landing_Outcome='Failure (drone ship)';",
    'Q9: 2015 drone ship failures by month'
)

# SQL 10: Rank landing outcomes between 2010-06-04 and 2017-03-20
sql(
    "SELECT Landing_Outcome, COUNT(*) AS Count FROM SPACEXTBL WHERE Date BETWEEN '2010-06-04' AND '2017-03-20' GROUP BY Landing_Outcome ORDER BY Count DESC;",
    'Q10: Landing outcome counts 2010-2017'
)

conn.close()
print('All 10 SQL queries complete!')
```
