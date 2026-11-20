# Delhi Weather and Air Quality Analysis (2025)

An exploratory data analysis of **hourly weather and air pollution data for Delhi in 2025** (52,560 rows, 6 stations). The project turns raw numbers into clear answers: when the air is cleanest, how often it broke safe limits, what kind of pollution it is, and which weather goes with dirty air.

It runs in one Google Colab notebook and produces **17 charts** and a table of **95 key parameters**.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/USERNAME/REPO/blob/main/Delhi_Analysis_Colab.ipynb)

## Questions answered
1. How hot, cold, humid and windy was Delhi, and when were the extremes?
2. Which months and hours had the worst and best air?
3. On how many days was the air within India's and WHO's safe limits?
4. On how many days would each GRAP stage (Delhi's emergency plan) have applied?
5. Which weather conditions go with dirty air, and which with clean air?
6. Is the pollution mostly smoke (traffic, burning) or dust?
7. Can the 365 days be grouped into a few simple types of day?
8. Did Diwali visibly change pollution?

## Key findings
| Measure | Zone A | Zone B |
|---|---|---|
| Annual mean PM2.5 (ug/m3) | 80.3 | 98.8 |
| Days above India's daily limit (60) | 244 | 270 |
| Days above WHO's daily guideline (15) | 365 | 365 |
| Worst month for PM2.5 | December | May |
| Cleanest month for PM2.5 | September | September |
| Days in Poor, Very Poor or Severe (CPCB) | 111 | 179 |

- **Winter is the worst season** (December and January): cold, still, foggy air traps smoke and traffic fumes. The monsoon is the cleanest, and September is the best month.
- **Night is worse than afternoon.** Across the city, PM2.5 peaks around 10 pm (about 110 ug/m3) and is lowest around 3 pm (about 64).
- **Two kinds of pollution.** Winter pollution is fine smoke-type particles (PM2.5/PM10 ratio about 0.96 to 0.99). Spring pollution, mainly in Zone B, is coarse dust (ratio about 0.13 to 0.24).
- **Rain cleans, fog traps.** Average PM2.5 is about 50 in rain or drizzle hours, about 94 in dry hours and about 193 in fog. All fog hours in the data are in December.
- **Four types of day** found by KMeans clustering: Cleanest (119 days), Moderate (107), Polluted, hot and dry (88) and Most polluted, cold and still (51).
- **No clear Diwali spike** appears in this data.

## Data
Hourly records from 1 Jan to 31 Dec 2025 at six stations: Anand Vihar, Connaught Place, Dwarka, IGI Airport, Okhla Phase III and Rohini.

| Type | Columns |
|---|---|
| Time and place | `date_ist`, `time_ist`, `location`, `lat`, `lon` |
| Weather | `temp_c`, `humidity`, `pressure_mb`, `windspeed_kph`, `condition_text`, `description` |
| Pollution | `aqi_index`, `pm2_5`, `pm10`, `co`, `no2` |

**Data source:** _add the source and licence of your dataset here._

### Data-quality notes
- **Pollution values are identical across stations in each group of three.** Anand Vihar, Connaught Place and Rohini match exactly; so do Dwarka, IGI Airport and Okhla. This suggests gridded or modelled data, so the stations are grouped into **Zone A** and **Zone B**. The project cannot name the single dirtiest station.
- **`aqi_index` is not usable as given.** It reaches 2,742, and 13.1% of rows are above 500, the top of the Indian scale. The project **recomputes the AQI from PM2.5** with the CPCB breakpoints.

## Project structure
```
.
├── Delhi_Analysis_Colab.ipynb        # the full analysis (14 cells)
├── data/
│   └── delhi-weather-aqi-2025.csv    # input data
├── charts/                           # the 17 output charts (PNG)
├── key_parameters.csv                # 95 key numbers (output of Cell 4)
├── docs/
│   └── Delhi_Project_Documentation.pdf   # full documentation with code and graphs
└── README.md
```

## How to run
**In Google Colab (recommended)**
1. Click the **Open In Colab** badge above.
2. Run the cells one by one, from top to bottom.
3. At Cell 2, click **Choose Files** and upload the CSV (it is skipped if the file is already in the folder).
4. Charts appear under each cell. Run Cell 14 to download everything as `results.zip`.

**Locally**
```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn jupyter
jupyter notebook Delhi_Analysis_Colab.ipynb
```
Skip Cell 14 locally, since it uses Colab-only features.

## Methods
| Step | What is done |
|---|---|
| Cleaning | Join date and time (day/month/year format), check for gaps and duplicates |
| Grouping | Six stations become two zones; build hourly and daily tables for each zone and for the whole city |
| AQI | PM2.5 converted to a CPCB AQI by linear interpolation between the official breakpoints, then to a category and an estimated GRAP stage |
| Patterns | Distributions, time series, monthly and seasonal averages, hour-of-day cycles, hour-by-month heatmaps |
| Relationships | Spearman correlations, PM2.5 against wind, humidity and temperature, weather condition against PM2.5 |
| Grouping of days | KMeans (4 clusters) on standardised weather and pollution, shown with PCA |

## Charts
| # | Chart |
|---|---|
| 1 | Headline numbers |
| 2 | Distribution of every parameter |
| 3 | Daily time series |
| 4, 5 | Monthly and seasonal averages |
| 6, 7 | Daily cycle and hour-by-month heatmaps |
| 8, 9 | Correlations and weather against PM2.5 |
| 10 | Weather conditions against PM2.5 |
| 11, 12 | CPCB categories, limit exceedance and AQI calendar |
| 13, 14, 15 | Worst days, PM2.5/PM10 ratio, station weather |
| 16, 17 | Four kinds of day and Diwali window |

_Add 2 or 3 of your best images here, for example:_
```markdown
![AQI calendar](charts/12_aqi_calendar.png)
![Day types](charts/16_weather_pollution_regimes.png)
```

## Limitations
- Only **one year** of data, so no long-term trends.
- Pollution values are copied across stations, so there are effectively **two** pollution series, not six.
- The AQI here is built from **PM2.5 only**. The official CPCB AQI uses the worst of several pollutants, so these are estimates.
- **GRAP days are estimates** based on AQI bands. They are not records of announced stages.
- Very large PM10 spikes in March to June were not verified against another source.
- Correlation does not prove causation, and the "dust" reading of spring pollution is an interpretation.

## Possible extensions
- Add 2023 and 2024 data to study trends.
- Use real station readings (for example CPCB or OpenAQ) to compare individual stations.
- Add SO2 and ozone for the full CPCB AQI.
- Forecast next-day PM2.5 with a time-based train and test split.
- Build a Streamlit dashboard.

## Tech stack
Python, pandas, NumPy, Matplotlib, Seaborn, SciPy, scikit-learn, Google Colab

## Author
**Your Name**: [GitHub](https://github.com/USERNAME) | [LinkedIn](https://www.linkedin.com/in/your-profile)

## License
Add a licence (for example MIT) for the code. Check the licence of the dataset separately before you redistribute it.
