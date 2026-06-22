# Interactive Map with Folium — Launch Sites

Author: Vishvi

```python
import folium
import pandas as pd
from folium.plugins import MarkerCluster

# Launch site coordinates
launch_sites = {
    'CCAFS LC-40':  {'lat': 28.5621, 'lon': -80.5774, 'success': 0.60},
    'CCAFS SLC-40': {'lat': 28.5620, 'lon': -80.5772, 'success': 0.71},
    'KSC LC-39A':   {'lat': 28.6080, 'lon': -80.6043, 'success': 0.77},
    'VAFB SLC-4E':  {'lat': 34.6332, 'lon':-120.6130, 'success': 0.57},
}

# Create base map centred on USA
site_map = folium.Map(location=[28.5, -80.5], zoom_start=5)

# Add markers for each launch site
for site, info in launch_sites.items():
    color = 'green' if info['success'] >= 0.70 else 'orange'
    folium.Marker(
        location=[info['lat'], info['lon']],
        popup=folium.Popup(
            '<b>' + site + '</b><br>Success Rate: ' +
            str(round(info['success']*100, 1)) + '%',
            max_width=200
        ),
        icon=folium.Icon(color=color, icon='rocket', prefix='fa')
    ).add_to(site_map)

    # Add proximity circle
    folium.Circle(
        location=[info['lat'], info['lon']],
        radius=1000,
        color=color,
        fill=True,
        fill_opacity=0.2
    ).add_to(site_map)

# Add launch outcome markers from dataset
url = 'https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DS0321EN-SkillsNetwork/datasets/dataset_part_2.csv'
df  = pd.read_csv(url)

site_coords = {
    'CCAFS LC-40':  [28.5621, -80.5774],
    'CCAFS SLC-40': [28.5620, -80.5772],
    'KSC LC-39A':   [28.6080, -80.6043],
    'VAFB SLC-4E':  [34.6332,-120.6130],
}

marker_cluster = MarkerCluster().add_to(site_map)

for _, row in df.iterrows():
    coords = site_coords.get(row['LaunchSite'])
    if coords:
        color  = 'green' if row['Class'] == 1 else 'red'
        folium.CircleMarker(
            location=coords,
            radius=5,
            color=color,
            fill=True,
            popup=row['LaunchSite'] + ' - ' + ('Success' if row['Class']==1 else 'Failure')
        ).add_to(marker_cluster)

site_map.save('launch_sites_map.html')
print('Map saved as launch_sites_map.html!')
site_map
```

## What the Map Shows

- Green markers = launch sites with 70%+ success rate
- Orange markers = launch sites below 70% success rate
- Clustered circles = individual launch outcomes
- Green circles = successful landing
- Red circles = failed landing
