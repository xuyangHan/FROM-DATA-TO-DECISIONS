# Some Figures for COVID-19 that Everybody Should Understand

In December 2019, the Chinese authorities notified the world that a virus was spreading through their communities. In the following months, it spread to other countries, with cases doubling within days. This virus is the Severe Acute Respiratory Syndrome-Related Coronavirus 2 that causes the disease called COVID-19, and that everyone simply calls coronavirus.

On March 11, 2020, the World Health Organization made an announcement:

> In the past two weeks, the number of cases of COVID-19 outside China has increased 13-fold. COVID-19 can be characterized as a pandemic.

By late May, this coronavirus had already overwhelmed almost all countries in the world. And this was still a warning to countries where it was spreading quickly. We still expected to see the number of cases, deaths, and affected countries climb even higher.

Developers, data analytics professionals, politicians, media, and the public all had an interest in answering questions about this COVID-19 pandemic:

- How severely were different countries hit by the coronavirus?
- How did different countries handle the situation?
- Were there projections for countries experiencing the epidemic, based on countries that already contained the spread?

In this article, I present some basic figures for COVID-19 that everybody should understand.

## Data Sources

Before getting started, here are some data sources that can help with analysis:

- [COVID-19 #CoronaVirus Data-Pack (Information is Beautiful)](https://docs.google.com/spreadsheets/d/1g_YxmDfQx7aOU2DKzNZo9b-NTk62Bju6X3z6OuCa6gw/edit#gid=1532031163)
- [Coronavirus Disease (COVID-19) - Statistics and Research (Our World in Data)](https://ourworldindata.org/coronavirus)
- [COVID-19 (Coronavirus) Data Resource Hub (Tableau)](https://www.tableau.com/covid-19-coronavirus-data-resources)
- [Johns Hopkins CSSE COVID-19 Dataset](https://github.com/CSSEGISandData/COVID-19)
- [Chinese Journal of Epidemiology](http://weekly.chinacdc.cn/en/article/id/e53946e2-c6c4-41e9-9a9b-fea8db1a8f51)
- [nCoV2019.live](https://ncov2019.live/data)

I used the data organized by Johns Hopkins University (JHU).

## Load and Explore the Data

```python
import pandas as pd
import requests
from bs4 import BeautifulSoup
import numpy as np

url_death = "https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_deaths_global.csv"
url_recovered = "https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_recovered_global.csv"
url_confirmed = "https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv"

df_confirmed = pd.read_csv(url_confirmed)
df_death = pd.read_csv(url_death)
df_recovered = pd.read_csv(url_recovered)

df_confirmed = df_confirmed.groupby("Country/Region").sum()
df_confirmed = df_confirmed.drop(columns=["Lat", "Long"])
df_death = df_death.groupby("Country/Region").sum()
df_death = df_death.drop(columns=["Lat", "Long"])
df_recovered = df_recovered.groupby("Country/Region").sum()
df_recovered = df_recovered.drop(columns=["Lat", "Long"])
```

By this point, confirmed, death, and recovered cases are loaded and grouped by country/region. Then active cases are calculated by `confirmed - death - recovered`.

```python
df_active = df_confirmed.copy()

# generate active cases for each country
for idx in df_recovered.index:
    for i in range(0, df_recovered.shape[1]):
        df_active.loc[idx][i] = df_confirmed.loc[idx][i] - df_death.loc[idx][i] - df_recovered.loc[idx][i]
```

Then a race chart visualization can be made by a web application called [Flourish](https://flourish.studio/). The CSV file was opened in Flourish and the visualization was exported to YouTube. The visualization (`1/22-3/27`) is shown below.

---

All the following charts are made by Plotly.py, a responsive visualization library in Python.

## 1) Confirmed Cases on a Map

```python
# visualize Covid 19 Confirmed Cases
import plotly.graph_objects as go
import pandas as pd

fig = go.Figure(
    data=go.Choropleth(
        locations=df_country["code"],
        z=df_country["confirmed"],
        colorscale="Reds",
        autocolorscale=False,
        reversescale=False,
        marker_line_color="darkgray",
        marker_line_width=0.5,
    )
)

fig.update_layout(
    title_text="Covid 19 Confirmed Cases",
    geo=dict(
        showframe=False,
        showcoastlines=False,
        projection_type="equirectangular",
    ),
    annotations=[
        dict(
            x=0.55,
            y=0.1,
            xref="paper",
            yref="paper",
            showarrow=False,
        )
    ],
)

fig.show()
```

By the same method, you can plot death, recovered, and active data as well.

## 2) Top 15 Countries Cases Overview

Secondly, we can plot data in a bar chart to check each country's status in containing the virus. The following bar chart shows 15 countries hit most by the coronavirus.

```python
# check top 15 overview
df_country_top15 = df_country.sort_values(by=["confirmed"], ascending=False).head(15)
x = df_country_top15["code"].tolist()
confirmed = df_country_top15["confirmed"].tolist()
death = df_country_top15["death"].tolist()
recovered = df_country_top15["recovered"].tolist()
active = df_country_top15["active"].tolist()

fig_cases = go.Figure(
    data=[
        go.Bar(x=x, y=active, name="active"),
        go.Bar(x=x, y=death, name="death"),
        go.Bar(x=x, y=recovered, name="recovered", text=confirmed, textposition="outside"),
    ]
)

# Change the bar mode
fig_cases.update_layout(
    barmode="stack",
    title="Top 15 Countries Cases Overview",
    xaxis=dict(
        title="Countries Codes",
        gridcolor="white",
        gridwidth=4,
    ),
    yaxis=dict(
        title="Cases",
        gridcolor="white",
        gridwidth=4,
    ),
    paper_bgcolor="rgb(243, 243, 243)",
    plot_bgcolor="rgb(243, 243, 243)",
)

fig_cases.show()
```

From this figure, you can directly see that some countries did not contain the virus very well and got overwhelmed. Some countries were still struggling, while others were almost recovered.

### In Detail

```python
df_country_top15["uncomplete"] = 1 - df_country_top15["recovered"] / (
    df_country_top15["active"] + df_country_top15["recovered"]
)

fig_complete = go.Figure(
    data=[
        go.Bar(x=x, y=df_country_top15["complete"], name="complete", marker_color="#66BB6A"),
        go.Bar(x=x, y=df_country_top15["uncomplete"], name="uncomplete", marker_color="#E57373"),
    ]
)

# Change the bar mode
fig_complete.update_layout(
    barmode="stack",
    title="Countries Task Complish",
    xaxis=dict(
        title="Countries Codes",
        gridwidth=0,
    ),
    yaxis=dict(
        title="Complete Rate",
        gridwidth=4,
    ),
    paper_bgcolor="rgb(243, 243, 243)",
    plot_bgcolor="rgb(243, 243, 243)",
)

fig_complete.show()
```

In countries like German, the majority of patients are recovered. And it's very obvious that either data in UK is inaccurate or UK is not following the cases if they got recovered.

## 3) Death Rate Estimates

Based on the public data, we can only estimate death rates. There are two ways to calculate the death rate:

1. If we look at solved cases (`recovered + death`), death rate is:
   - `death / (death + recovered)`
   - This can overestimate the death rate, especially for countries that just started reacting.
2. If we look at all cases (including unsolved cases), death rate is:
   - `death / confirmed`
   - This can underestimate the death rate because it assumes all active cases will recover later.

So, the real death rate should be somewhere in the middle.

```python
# check top 15 death Rates
df_country_top15["complete"] = df_country_top15["recovered"] / (
    df_country_top15["active"] + df_country_top15["recovered"]
)
df_country_top15["solved death rate"] = df_country_top15["death"] / (
    df_country_top15["death"] + df_country_top15["recovered"]
)
df_country_top15["lowest possible death rate"] = df_country_top15["death"] / (
    df_country_top15["confirmed"]
)

fig_deathRate = go.Figure(
    data=[
        go.Scatter(
            x=x,
            y=df_country_top15["lowest possible death rate"],
            mode="lines",
            marker_size=df_country_top15["confirmed"] / 1000,
            name="unsolved death rate",
        ),
        go.Scatter(
            x=x,
            y=df_country_top15["solved death rate"],
            mode="lines",
            marker_size=df_country_top15["confirmed"] / 1000,
            name="solved death rate",
        ),
    ]
)

fig_deathRate.update_layout(
    title="Countries Death Rates",
    xaxis=dict(
        title="Countries Codes",
        gridcolor="white",
        gridwidth=4,
    ),
    yaxis=dict(
        title="Death Rates",
        gridcolor="white",
        gridwidth=4,
    ),
    paper_bgcolor="rgb(243, 243, 243)",
    plot_bgcolor="rgb(243, 243, 243)",
)

fig_deathRate.show()
```

Seeing data from China, which is the first outbreak country, the death rate seemed close to 4%. Countries with high death rates might indicate weaker handling of the situation, including the US, Italy, Spain, France, and the UK.

## 4) Infection Trajectories (Log Scale)

Another commonly used figure is the line chart to track confirmed cases over days on a log scale. At the beginning phases, confirmed cases grow exponentially and look like a straight line in a log-scale chart. Whenever the curve starts to bend, it means social distancing is working.

```python
# get top 50 countries data
df_country_top50_confirmed = df_confirmed.sort_values(by=[today], ascending=False).head(50)
fig_trajectories = go.Figure()

for i in range(0, df_country_top50_confirmed.shape[0]):
    data = df_country_top50_confirmed.iloc[i]
    data = data[data > 150]
    trace = go.Scatter(
        x=np.array(range(0, data.shape[0])),
        y=data,
        name=data.name,
        mode="lines",
    )
    fig_trajectories.add_trace(trace)

fig_trajectories.update_layout(
    title="Countries Virus Infection Trajectories",
    xaxis=dict(
        title="Days after 150 Confirmed Cases",
        gridcolor="white",
        gridwidth=4,
    ),
    yaxis=dict(
        title="Confirmed Cases",
        gridcolor="white",
        type="log",
        gridwidth=4,
    ),
    paper_bgcolor="rgb(243, 243, 243)",
    plot_bgcolor="rgb(243, 243, 243)",
)

fig_trajectories.show()
```

## 5) Daily New Cases Curve

When everyone talks about "flattening the curve", the curve they are referring to is daily confirmed cases.

```python
# get top 50 countries data
df_country_top15_confirmed = df_confirmed.sort_values(by=[today], ascending=False).head(50)
fig = go.Figure()

for i in range(0, df_country_top15_confirmed.shape[0]):
    data = df_country_top15_confirmed.iloc[i]
    data = data[data > 150]
    data_daily = []
    for j in range(1, data.shape[0]):
        data_daily.append(data[j] - data[j - 1])

    trace = go.Scatter(
        x=np.array(range(0, len(data_daily))),
        y=data_daily,
        name=data.name,
        mode="lines",
    )

    fig.add_trace(trace)

fig.update_layout(
    title="Countries Virus Daily New Cases Curve",
    xaxis=dict(
        title="Days after 150 Confirmed Cases",
        gridcolor="white",
        gridwidth=4,
    ),
    yaxis=dict(
        title="Daily Confirmed Cases",
        gridcolor="white",
        gridwidth=4,
    ),
    paper_bgcolor="rgb(243, 243, 243)",
    plot_bgcolor="rgb(243, 243, 243)",
)

fig.show()
```

Each country has its own medical capacity. If daily cases are higher than that capacity, the system can get overwhelmed, and more confirmed cases may follow as a sequence. You can notice that curves in some countries do not look flattened at all.

## Final Thoughts

As a summary, the above are some figures you should understand for COVID-19. The pandemic is a huge challenge for humankind. Some countries already showed this was not an impossible war to win. What we need is to learn from countries that successfully contained the virus and stand together.

Even though the turning point could not be seen at that time, I did have faith in humankind.

**Be stronger together.**