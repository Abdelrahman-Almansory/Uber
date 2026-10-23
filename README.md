# Uber Trip Analysis | Power BI Dashboard

An interactive, three-page Power BI report analyzing ~104K Uber-style ride bookings from June 2024. It tracks booking trends, revenue, trip efficiency, demand by time, and location patterns, so a business stakeholder can see where demand is, when it peaks, and which vehicle types carry the volume.

**Tools:** Power BI Desktop, DAX, Power Query

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Business Questions](#business-questions)
3. [Dataset](#dataset)
4. [Dashboard Walkthrough](#dashboard-walkthrough)
5. [Key Insights](#key-insights)
6. [Technical Highlights](#technical-highlights)
7. [Data Model](#data-model)
8. [Repository Structure](#repository-structure)
9. [How to Open the Report](#how-to-open-the-report)
10. [Limitations](#limitations)
11. [Future Improvements](#future-improvements)
12. [Acknowledgments](#acknowledgments)

---

## Project Overview

Ride-hailing operators make daily decisions about driver supply, pricing, and vehicle mix. This project turns raw trip records into a decision-ready report that answers those questions at a glance.

The report is split into three pages:

| Page | Purpose |
|------|---------|
| **Overview Analysis** | Headline KPIs, payment and day/night split, vehicle type grid, daily revenue trend, and location analysis |
| **Time Analysis** | Demand by pickup time, day of week, and an hour-by-day heatmap |
| **Details** | Trip-level grid for drilling into the records behind any chart |

Every page has **Date** and **City** slicers, and the main charts respond to a **measure selector** (Total Bookings, Total Booking Value, Total Trip Distance).

---

## Business Questions

- How many trips were booked, and how much revenue did they generate?
- What does a typical trip look like in distance, duration, and value?
- Which payment methods and vehicle types dominate?
- When is demand highest and lowest, by hour and by day of week?
- Where do trips start and end most often, and which vehicle do riders prefer at each pickup point?
- What are the outlier trips, such as the longest distance?

---

## Dataset

| Property | Detail |
|----------|--------|
| Records | ~103.7K bookings |
| Period | 1 June 2024 to 30 June 2024 |
| Geography | New York City-style pickup and drop-off zones |
| Source | [add dataset source / link] |

Fields used in the report include pickup and drop-off location, vehicle type, payment type, pickup date and time, booking value, trip distance, and trip time.

> **Note:** This appears to be a sample or simulated dataset (for example, the average booking value is about $15 across all vehicle types). The findings below describe this dataset only and should not be read as statements about real Uber operations.

---

## Dashboard Walkthrough

### 1. Overview Analysis

<img width="1415" height="802" alt="image" src="https://github.com/user-attachments/assets/beecb5da-7807-4563-ae32-ee9f7cd99c58" />


- **KPI cards:** Total Bookings, Total Booking Value, Avg Booking Value, Total Trip Distance, Avg Trip Distance, Avg Trip Time
- **Donut charts:** Bookings by payment type and by day/night trips, switchable through the measure selector
- **Vehicle Type Analysis grid:** Bookings, booking value, average booking value, and distance per vehicle type, with in-cell data bars
- **Total Booking Value by Day:** Daily revenue trend for the selected period
- **Location Analysis:** Most frequent pickup and drop-off points, farthest trip, top 5 locations by bookings, and the most preferred vehicle at pickup

### 2. Time Analysis
<img width="1421" height="798" alt="image" src="https://github.com/user-attachments/assets/c82c1119-8785-4937-918c-1e35935e486e" />


- **Bookings by Pickup Time:** Area chart of demand across the day
- **Bookings by Day Name:** Line chart comparing days of the week
- **Hour and Day Heatmap:** Matrix grid (hours 0 to 23 by days of the week) with color intensity showing demand, plus an hourly bar summary

### 3. Details
<img width="1422" height="802" alt="image" src="https://github.com/user-attachments/assets/da9d63fa-1ce2-4730-85d3-0d2059dde42c" />


A trip-level grid with key fields, for exploring the records behind a selected chart or filter.

---

## Key Insights

### Headline KPIs

| Metric | Value |
|--------|-------|
| Total Bookings | 103.7K |
| Total Booking Value | $1.6M |
| Average Booking Value | $15.0 |
| Total Trip Distance | 349K miles |
| Average Trip Distance | 3 miles |
| Average Trip Time | 16 minutes |

### Vehicle Types

| Vehicle | Bookings | Booking Value | Distance (miles) |
|---------|---------:|--------------:|-----------------:|
| UberX | 38,744 | $583,880 | 131,496 |
| Uber Comfort | 17,078 | $253,995 | 56,790 |
| Uber Black | 16,710 | $250,192 | 56,149 |
| UberXL | 16,698 | $249,424 | 55,721 |
| Uber Green | 14,498 | $216,181 | 48,778 |

- **UberX carries the volume.** It accounts for about 37% of bookings and about 36% of booking value, more than double any other vehicle type.
- **Average booking value is about $15 for every vehicle type**, which points to a synthetic or heavily normalized fare field.

### Payments and Trip Type

- **Uber Pay** handled about 67% of bookings (70K) and **cash** about 32% (33K). Amazon Pay and Google Pay were negligible.
- **Day trips** made up about 65% of bookings (68K) versus about 35% at night (36K).

### Demand Over Time

- **Daily peak:** Demand climbs from the early morning, peaks in the mid-afternoon, and falls to its lowest point around 3 to 4 AM.
- **Weakest days:** Friday (9.3K) and Thursday (11.2K) have the fewest bookings.
- **Calendar effect to be aware of:** Sunday (19.2K) and Saturday (18.7K) have the highest totals, but June 2024 contains five of each and only four of every other weekday. Averaged per day, Sunday, Saturday, Monday, Tuesday, and Wednesday are roughly level at about 3.7K to 3.9K bookings. The clear finding is the Thursday and Friday dip, not a weekend surge.

### Locations

- **Most frequent pickup:** Penn Station/Madison Sq West
- **Most frequent drop-off:** Upper East Side North
- **Top pickup zones** are Penn Station/Madison Sq West and the Upper East Side zones, followed by Lenox Hill and the Upper West Side.
- **UberX is the most preferred vehicle for pickup** (about 39K bookings).
- **Farthest trip:** Lower East Side to Crown Heights North at 144.1 miles, a clear outlier against a 3-mile average, worth treating as a data-quality check.

---

## Technical Highlights

- **Measure selector with a disconnected table:** A standalone table holds the three measure names, and a `SWITCH`-based DAX measure returns the selected metric, so one set of visuals can show bookings, value, or distance.
- **Dynamic chart titles** that update with the selected measure.
- **Inactive relationship for drop-offs:** A second relationship between the trip table and the location table is activated with `USERELATIONSHIP` to analyze drop-off points alongside pickups.
- **Time intelligence helpers:** Pickup hour, day name, and day/night classification, with day names sorted in calendar order.
- **Conditional formatting and data bars** in the vehicle grid and heatmap.
- **Custom navigation** with page buttons and a sidebar, plus Date and City slicers on every page.

---

## Data Model

A star-style model with a central trips fact table connected to dimension tables for locations, vehicle types, payment types, and dates. Pickup and drop-off both relate to the location table, with pickup as the active relationship and drop-off activated on demand in DAX. The measure selector table is intentionally disconnected.
---

## How to Open the Report

1. Install [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free, Windows).
2. Download or clone this repository.
3. Open `Uber_Trip_Analysis.pbix`.
4. If prompted, point the data source to the dataset in the `data/` folder and refresh.
5. Use the Date and City slicers and the measure selector to explore.

---

## Limitations

- The dataset covers a single month, so seasonality cannot be assessed.
- The data appears simulated, so pricing and demand findings are illustrative.
- Some outliers, such as the 144-mile trip, have not been cleaned or removed.
- No external factors such as weather, holidays, or events are included.

---

## Future Improvements

- Normalize day-of-week comparisons by the number of occurrences in the period
- Add outlier detection and a data-quality page
- Extend to multiple months for trend and seasonality analysis
- Add a forecasting view for demand by zone and hour
- Publish to the Power BI Service with scheduled refresh
