# Bellabeat: How People Use Smart Devices

Analysis of Fitbit activity and sleep data to guide Bellabeat’s marketing strategy, using MySQL and Power BI.

**Author:** Gbenga Rahman A.
**Tools:** MySQL · Power BI · PowerPoint · Word document 

-----

## Project overview

Bellabeat is a high-tech manufacturer of health-focused smart products for women. Its cofounder and Chief Creative Officer, Urška Sršen, believes smart device usage data can reveal new growth opportunities.

This project analyzes how people use non-Bellabeat smart devices, then applies those trends to one Bellabeat product. The recommendations are for the Bellabeat app, backed by its membership program.

## Business task

Find trends in how people use smart devices and turn them into high-level marketing recommendations for one Bellabeat product.

Questions answered:

1. What are some trends in smart device usage?
1. How could these trends apply to Bellabeat customers?
1. How could these trends help influence Bellabeat’s marketing strategy?

**Stakeholders:** Urška Sršen (cofounder, Chief Creative Officer), Sando Mur (cofounder), and the Bellabeat marketing analytics team.

## Data

|Item         |Details                                                            |
|-------------|-------------------------------------------------------------------|
|Source       |FitBit Fitness Tracker Data (Kaggle, made available through Mobius)|
|License      |CC0: Public Domain                                                 |
|Files used   |`dailyActivity_merged.csv`, `sleepDay_merged.csv`                  |
|Period       |12 April 2016 to 12 May 2016 (31 days)                             |
|Activity data|940 rows, 33 users                                                 |
|Sleep data   |413 rows, 24 users                                                 |

The raw CSV files are not included in this repository. Search for “FitBit Fitness Tracker Data” by Mobius on Kaggle to download them.

## Process

The work follows the six steps of the data analysis process: Ask, Prepare, Process, Analyze, Share, Act.

### Data cleaning (MySQL)

|Step             |What was done                                            |Result                                           |
|-----------------|---------------------------------------------------------|-------------------------------------------------|
|Import           |Loaded both CSVs into raw tables, with dates kept as text|940 activity rows, 413 sleep rows                |
|Duplicates       |Grouped by user and date to find repeats                 |Activity: none. Sleep: 3 user-days appeared twice|
|Remove duplicates|Built the clean sleep table with `DISTINCT`              |413 rows down to 410                             |
|Fix dates        |Converted text dates to real dates with `STR_TO_DATE`    |Both tables run 2016-04-12 to 2016-05-12         |
|Zero-step days   |Removed days with 0 steps, most likely tracker not worn  |77 days removed, 940 rows down to 863            |
|Join             |Joined activity and sleep on user and date               |410 matched nights, 24 users                     |

The original tables were never edited. All work was done on clean copies.

### SQL techniques used

`GROUP BY` and `HAVING` for duplicate checks, `CASE` for user segments, CTEs (`WITH`) for per-user averages, subqueries for percentages, `INNER JOIN` on user and date, date functions (`STR_TO_DATE`, `DAYNAME`, `DAYOFWEEK`), and a hand-written Pearson correlation formula.

## Key findings

- **Mostly sedentary:** users are sedentary for 956 minutes a day (66.4% of the day). Very active and fairly active time together make up 2.6%.
- **Modest steps:** the average is 8,319 steps a day. Only 7 of 33 users (21.2%) average 10,000 steps or more, and 16 of 33 (48.5%) average under 7,500.
- **Activity and calories:** active users (10,000+ steps) burn about 19% more calories a day than sedentary users (2,578 against 2,161). Steps and calories have a moderate positive correlation of 0.562.
- **Strong engagement:** 25 of 33 users (75.8%) logged activity on at least 21 of the 31 days.
- **Weekly rhythm:** steps peak on Tuesday (8,949) and Saturday (8,947) and are lowest on Sunday (7,627). Sleep is shortest on Tuesday and Thursday (6.7 hours) and longest on Sunday (7.5 hours).
- **Sleep:** average sleep is 7.0 hours, sleep efficiency is 91.4%, users lie awake about 39 minutes a night, and 44.1% of nights are under 7 hours.
- **Sleep tracking gap:** 9 of 33 users (27.3%) have no sleep records.
- **Activity and sleep:** higher-step days were followed by shorter sleep (454 minutes under 5,000 steps, 396 minutes at 10,000 or more). The link is weak (correlation -0.19), so it shows a pattern, not a cause.

## Recommendations

Focus on the **Bellabeat app**, supported by membership, with three campaigns:

1. **Make small movement the message.** Movement reminders, short daily challenges, goals just above each user’s own average, and nudges on Friday, Sunday and Monday.
1. **Coach sleep around the 7-hour mark.** Bedtime reminders, wind-down content, a weekly sleep summary, and a midweek push promoting membership sleep guidance.
1. **Get users wearing the device overnight and keep them engaged.** Overnight-wear onboarding (Leaf can be worn as a bracelet, necklace or clip), streaks and weekly summaries, and re-engagement for users with moderate or low use.

## Dashboard

The Power BI dashboard is included in this repository as `dashboard/Bellabeat_Dashboard.pbix`. Open it in Power BI Desktop (free) to explore the visuals.

**Activity page:** average steps, share of day sedentary, users analyzed, share of the day by activity level, users by step segment, and users by days logged.

**Sleep page:** average hours asleep, share of nights under 7 hours, steps by day of week, sleep by day of week, sleep by step group, and a nightly scatter of steps against minutes asleep.

## Repository contents

```
.
├── README.md
├── presentation/
│   └── Bellabeat_Smart_Device_Analysis.pptx
├── dashboard/
│   └── Bellabeat_Dashboard.pbix
└── sql/
    └── bellabeat_analysis.sql
```

## Limitations

- Small sample: 33 users for activity and 24 for sleep.
- Short, old data: 31 days in spring 2016.
- No age, gender or location data, and Bellabeat’s customers are women.
- The data comes from Fitbit users, not Bellabeat customers.
- Some very low-step days remain after removing zero-step days and may be partial-wear days.
- Correlations show patterns, not causes.

## Author

**Gbenga Rahman A.**
