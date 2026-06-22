# Plotly Dash Dashboard — SpaceX Launch Analysis

Author: Vishvi

Save this as spacex_dash.py and run with: python spacex_dash.py

```python
import pandas as pd
import dash
from dash import dcc, html
from dash.dependencies import Input, Output
import plotly.express as px

# Load dataset
url = 'https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DS0321EN-SkillsNetwork/datasets/spacex_launch_dash.csv'
df  = pd.read_csv(url)
max_payload = df['Payload Mass (kg)'].max()
min_payload = df['Payload Mass (kg)'].min()

app = dash.Dash(__name__)

app.layout = html.Div([
    html.H1('SpaceX Falcon 9 Launch Dashboard',
            style={'textAlign':'center','color':'#503D36','fontSize':30}),

    # Dropdown - launch site selector
    dcc.Dropdown(
        id='site-dropdown',
        options=[
            {'label':'All Sites', 'value':'ALL'},
            {'label':'CCAFS LC-40',  'value':'CCAFS LC-40'},
            {'label':'CCAFS SLC-40', 'value':'CCAFS SLC-40'},
            {'label':'KSC LC-39A',   'value':'KSC LC-39A'},
            {'label':'VAFB SLC-4E',  'value':'VAFB SLC-4E'},
        ],
        value='ALL',
        placeholder='Select a Launch Site',
        searchable=True
    ),

    html.Br(),
    html.Div(dcc.Graph(id='success-pie-chart')),
    html.Br(),

    html.P('Payload range (Kg):'),
    dcc.RangeSlider(
        id='payload-slider',
        min=0, max=10000, step=1000,
        value=[min_payload, max_payload],
        marks={i: str(i) for i in range(0, 11000, 2500)}
    ),

    html.Div(dcc.Graph(id='success-payload-scatter'))
])

@app.callback(
    Output('success-pie-chart', 'figure'),
    Input('site-dropdown', 'value')
)
def update_pie(selected_site):
    if selected_site == 'ALL':
        fig = px.pie(
            df, values='class', names='Launch Site',
            title='Total Success Launches by Site'
        )
    else:
        filtered = df[df['Launch Site'] == selected_site]
        fig = px.pie(
            filtered, names='class',
            title='Success vs Failure for ' + selected_site
        )
    return fig

@app.callback(
    Output('success-payload-scatter', 'figure'),
    [Input('site-dropdown', 'value'),
     Input('payload-slider', 'value')]
)
def update_scatter(selected_site, payload_range):
    low, high = payload_range
    filtered  = df[(df['Payload Mass (kg)'] >= low) &
                   (df['Payload Mass (kg)'] <= high)]
    if selected_site != 'ALL':
        filtered = filtered[filtered['Launch Site'] == selected_site]
    fig = px.scatter(
        filtered, x='Payload Mass (kg)', y='class',
        color='Booster Version Category',
        title='Payload Mass vs Landing Success'
    )
    return fig

if __name__ == '__main__':
    app.run_server(debug=True)
```

## Dashboard Features

| Feature | Description |
|---|---|
| Site dropdown | Select All Sites or individual site |
| Pie chart | Success vs failure for selected site |
| Payload slider | Filter by payload mass range |
| Scatter chart | Payload vs success coloured by booster |
