# 🦠 Coronavirus (COVID-19) Live Tracker Dashboard in Power BI

**A live Power BI dashboard that pulls the Worldometer coronavirus table straight from the web, tracks cases, deaths, recoveries, and critical cases for every country, and refreshes itself 8 times a day through the Power BI Service and an on-premises gateway.**

## Dashboard Preview

![Homepage](Dashboard%20Homepage.PNG)

![Overview of Coronavirus](dashboard%20overview.PNG)

![Country Wise Information of Virus](country%20wise%20data%20image.png)

![Country-Wise Comparison](detail%20comparison%20image.png)

![Comparison between New Cases](new%20cases%20image.png)

![New Death Cases by Country with Drillthrough](geographical%20new%20cases%20with%20drillthrough%20image.png)

![Summary](Data%20Summary%20table%20image.png)

![Drillthrough Table](drillthrough%20table%20image.png)

## Project Overview

In early 2020 the coronavirus numbers changed every day, and there was no single place to see them clearly. This project builds a tracker that answers the daily questions people were asking:

- How many cases, recoveries, deaths, and serious cases are there worldwide right now?
- Which countries have the most deaths, the most active cases, and the most serious cases?
- Where are **new** cases and **new** deaths appearing today?
- What share of a country's cases recovered, died, or are critical?
- When was the data last refreshed?

The data comes from a **web page, not a file**, so the dashboard always shows the latest numbers after each refresh.

## Snapshot of the Numbers

Values below are from the dashboard refresh on **09 April 2020, 17:13** (as shown on the Homepage and Overview pages):

| KPI | Value |
|---|---|
| Total cases | **1,530,689** |
| Active cases | **1,103,355** |
| Total recovered | **337,895** |
| Total deaths | **89,439** |
| Total serious / critical | **48,213** |
| New cases (today) | **12,008** |
| New deaths (today) | **971** |

**What the numbers say, in simple words** (shares calculated from the totals above)

- About **72%** of all cases were still active, about **22%** had recovered, and about **6%** had died.
- About **3%** of all cases were serious or critical.
- **Italy** had the most deaths (17,669), followed by **Spain** (15,238), **USA** (14,797), **France** (10,869), and **UK** (7,097).
- The **USA** had the most total cases (435,160), the most active cases (397,472), and the most serious cases (9,279).
- **China** had the most recovered cases (77,370).
- **Spain** reported the most new cases (4,226) and the most new deaths (446) in a single day.
- Of all new reports that day, about **7.5%** were deaths and **92.5%** were cases.
- The two cruise ships had their own numbers: **Diamond Princess** (82 active, 11 deaths, 619 recovered, 10 serious) and **MS Zaandam** (7 active, 2 deaths).

## Project Workflow

```
Worldometer web page
        ↓  Get Data > From Web
Power Query (light cleaning, data types)
        ↓
Data model + DAX measures (KPIs and percentages)
        ↓
8 report pages (cards, charts, map, tables, drillthrough)
        ↓  Publish
Power BI Service (workspace, report, dashboard)
        ↓  On-premises data gateway
Scheduled refresh, 8 times a day
```

## Data Source

- **Website:** [Worldometer Coronavirus](https://www.worldometers.info/coronavirus/)
- **Connection:** *Get Data > Web*, opened as *HTML Page (Legacy)*.
- **Table loaded:** the country-wise coronavirus table, named `Coronavirus Data` in the model (212 rows).

![Data Source Settings](data%20source%20image.png)

### What the Data Looks Like

![Data Table](data%20table%20image.png)

| Column | Meaning |
|---|---|
| `Countries` | Country, territory, or cruise ship name |
| `Total Cases`, `New Cases` | All confirmed cases, and cases reported today |
| `Total Deaths`, `New Deaths` | All deaths, and deaths reported today |
| `Active Cases` | Cases still being treated |
| `Total Recovered` | Recovered patients |
| `Total Serious, Critical` | Patients in serious or critical condition |
| `Tot Cases/ 1M pop` | Total cases per 1 million people |
| `Deaths/ 1M pop` | Deaths per 1 million people |
| `Total Tests`, `Tests/ 1M pop` | Tests done, and tests per 1 million people |

## Data Preparation in Power Query

The web table needed only light cleaning:

1. Connected to the Worldometer page and selected the country table.
2. Fixed the data format of each column. Number columns became whole numbers, and `Countries` was set as a geographic (Country/Region) field so the maps can plot it.
3. Created a small `Refresh` table that stores the date and time of the last refresh, shown on the dashboard as **Last Refresh**.
4. Closed and applied the changes, then built the model on top.

## Measures and KPIs

All measures sit in the `Coronavirus Data` table.

| Measure | What it gives |
|---|---|
| `Total Cases`, `Total Deaths`, `Total Recovered`, `Active Cases`, `Total Serious, Critical` | Headline KPI totals for the cards and charts |
| `New Cases`, `New Deaths` | Today's new reports |
| `Total Countries` | Number of countries in the data |
| `% Recovered` | Recovered ÷ Total cases |
| `% Dead` | Deaths ÷ Total cases |
| `% Critical` | Serious/critical ÷ Total cases |
| `Last Refresh` | Date and time of the latest data refresh |

**Example (USA, from the drillthrough page):** 22,891 recovered out of 435,160 total cases gives **5.3% recovered**. 14,797 deaths gives **3.4% dead**. 9,279 serious cases gives **2.1% critical**.

## Dashboard Pages

The report has **8 pages**, linked by a navigation bar, buttons, and bookmarks.

| # | Page | What it shows |
|---|---|---|
| 1 | **Homepage** | Cover page with title, *Last Refresh* card, and navigation buttons (info, home, globe, menu, Twitter) |
| 2 | **Overview of Coronavirus** | 6 KPI cards (total, recovered, deaths, active, new cases, serious), a country-wise death column chart, and a heat-colored table of active, recovered, dead, and serious cases |
| 3 | **Country Wise Information of Virus** | Bubble map where circle size shows deaths, a *Country Names* slicer, and separate cards for the Diamond Princess and MS Zaandam cruise ships |
| 4 | **Country-Wise Comparison** | Data-bar table, donut chart of case types, top-10 comparisons (deaths vs recovered, total vs active, serious vs active), and area charts for the highest deaths, recoveries, and serious cases |
| 5 | **Com b/w New Case** | New deaths and new cases: bar charts, a donut chart, combined line chart, and tables for the top countries |
| 6 | **New Death Cases** | Filled map of new deaths by country. Hover for details, right-click to drill through |
| 7 | **Summary** | Full data table with heat colors, a *Country Names* slicer, and 3 KPI trend cards: Recovered %, Dead %, and Critical % |
| 8 | **Drillthrough** | Country detail table with % Recovered, % Dead, % Critical, new cases, and new deaths, plus home and back buttons |

### Drillthrough in Action

Right-click a country on the *New Death Cases* map and choose **Drillthrough** to open that country's full detail table. For example, India shows 8 new deaths, 321 new cases, and 167,235 total tests in the tooltip.

## Design Choices

- **Dark theme** so the red, green, and orange numbers stand out.
- **Color meaning stays the same on every page:**

| Meaning | Color |
|---|---|
| Recovered | Green `#8ACA2B` |
| Deaths | Red `#CC0A15` |
| Serious / critical | Orange `#FF9900` |
| Active cases | Purple-blue |
| Total cases | White |

- **Background images** (virus graphics and medical photos) inside charts for a themed look.
- **Navigation icons** from [icons8.com](https://icons8.com): home, geography, content, and Twitter.
- **Bookmarks** for *homepage*, *overview*, *summary*, and *Map*, used by the navigation buttons.
- **Indian number format** on the dashboard (for example 15,30,689).

## Publishing and Auto Refresh

1. **Published** the report from Power BI Desktop to the **Power BI Service** (cloud) workspace.
2. Created a **dashboard** on the cloud by pinning the key visuals, and checked the **mobile view**.
3. Installed an **on-premises data gateway** on my local computer so the cloud could reach the data source.
4. Set a **scheduled refresh** of **8 times a day**, which was the limit available on the plan I was using.

### Lesson Learned

The gateway on my own computer only works while that computer is **switched on, connected to the internet, and signed in** at every refresh time. When my system was off, the refresh failed.

> **Takeaway:** For any real project, install the gateway on an always-on **server**, not on a personal laptop or desktop. Use the *standard* gateway for shared or production reports, because the *personal* gateway is an interactive app meant for one person's own computer.

## What I Learned

- Building reports in **Power BI Desktop**: data model, DAX measures, formatting, bookmarks, and drillthrough.
- Connecting to data from the **web** and cleaning it in Power Query.
- Working with the **Power BI Service**: workspaces, publishing, cloud dashboards, and mobile view.
- Setting up an **on-premises gateway** and **scheduled refresh**, and why a server is the right place for it.
- How Power BI **licenses and limits** differ (table below).

### Power BI Limits to Remember

Checked against Microsoft's documentation:

| Topic | Limit |
|---|---|
| Scheduled refreshes per day, **Power BI Pro** | Up to **8** |
| Scheduled refreshes per day, **Premium Per User** and **Premium / Fabric capacity** | Up to **48** |
| Default semantic model (dataset) size | **1 GB** |
| Model upload from Power BI Desktop | **10 GB** |
| Models above 10 GB | Need the **large semantic model storage format**, available on Fabric / Premium capacity, Premium Per User, and reserved Pro capacity |
| Gateway, personal mode | One per user, Import models only, runs as an app on your computer |
| Gateway, standard mode | For servers and teams, supports DirectQuery and live connections |

Sources: [Scheduled refresh](https://learn.microsoft.com/power-bi/refresh-scheduled-refresh), [Large semantic models](https://learn.microsoft.com/power-bi/enterprise/service-premium-large-models), [Personal gateway](https://learn.microsoft.com/power-bi/connect-data/service-gateway-personal-mode). Licensing changes over time, so check Microsoft's pages before planning a project.

## Limitations

- The dashboard depends on the **layout of the Worldometer web page**. If the site changes its table, the query needs to be updated.
- The screenshots show **one moment in time (9 April 2020)**. The repo stores the report file, not a live link.
- Worldometer figures are reported by countries and change often, so treat the numbers as indicative.

## Files Included

| File | Description |
|---|---|
| `Coronavirus Dashboard tracker.pbix` | Completed Power BI dashboard (data model, measures, 8 pages) |
| `coronavirus dashboard color codes.txt` | Theme color codes (green, red, orange) |
| `Dashboard Homepage.PNG` | Homepage preview |
| `dashboard overview.PNG` | KPI overview page |
| `country wise data image.png` | Country map page |
| `detail comparison image.png` | Country comparison page |
| `new cases image.png` | New cases and deaths page |
| `geographical new cases with drillthrough image.png` | New death map with drillthrough tooltip |
| `Data Summary table image.png` | Summary page |
| `drillthrough table image.png` | Drillthrough page |
| `data table image.png` | Data table in the model |
| `data source image.png` | Web data source settings |
| `README.md` | Project information |

## How to Use

1. Download `Coronavirus Dashboard tracker.pbix`.
2. Open it with **Power BI Desktop**.
3. Click **Refresh** to pull the latest table from the Worldometer page (if the page layout is unchanged).
4. Use the **Country Names** slicers, the map, and the navigation buttons to explore. Right-click a country on the map to drill through.

## Tools and Skills

- Power BI Desktop (web connector, Power Query, data modelling)
- DAX measures and KPIs
- Interactive visuals: maps, slicers, tooltips, drillthrough, bookmarks
- Power BI Service: workspaces, dashboards, mobile view
- On-premises data gateway and scheduled refresh
- Dashboard design and data storytelling

## Data Source

[Worldometer](https://www.worldometers.info/coronavirus/) COVID-19 country table. Icons from [icons8.com](https://icons8.com).

## Author

**Minhaj Khan**
Data Analyst | SQL · Power BI · Excel · UI/UX Designer · Python

[LinkedIn](https://www.linkedin.com/in/minhaj-khan/) · [Portfolio](https://github.com/minhajprince)
