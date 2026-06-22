# EDA and Interactive Visual Analytics Methodology

Author: Vishvi

---

## EDA Methodology

### Objective
Understand patterns in the SpaceX launch data before modelling.
Identify which features most influence landing success.

### Tools Used

| Tool | Purpose |
|---|---|
| pandas | Data manipulation and groupby analysis |
| matplotlib | Static visualisations |
| seaborn | Statistical plots and heatmaps |
| SQL (sqlite3) | Querying launch data with SQL |

### Approach

1. Analysed launch success rates by site, orbit, payload
2. Studied yearly trends in landing success
3. Explored correlations between numerical features
4. Used SQL to answer specific business questions

---

## Interactive Visual Analytics Methodology

### Folium (Interactive Maps)

| What | How |
|---|---|
| Map all launch sites | folium.Marker() at each site location |
| Colour by success | Green = success, Red = failure |
| Site proximity | folium.Circle() radius markers |

### Plotly Dash (Dashboard)

| Component | What it shows |
|---|---|
| Dropdown | Select launch site |
| Pie chart | Success vs failure for selected site |
| Range slider | Filter by payload mass |
| Scatter chart | Payload vs success correlation |

---

*Built by Vishvi | IBM Data Science Capstone*
