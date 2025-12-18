# Wind Mechanisms Dashboard

One-page flexdashboard to show how wind direction and speed relate to PM2.5.

## Purpose

- Visualize directional fingerprints of pollution (“north cleans, south accumulates”).
- Compare winter vs summer wind roses.
- Quantify how PM2.5 changes with wind direction and average wind speed.

## What’s inside

- **Pollution rose (all periods)**: frequency by wind direction (16-point) with AQI-level colors.
- **Winter vs Summer pollution roses**: seasonal contrast of directional pollution patterns.
- **Mean PM2.5 by wind direction (fill = mean wind speed)**: interactive ggplotly bar chart; tooltips show full direction name, mean PM2.5, and mean wind speed.

## Data inputs

- `beijing_air_processed.csv` (required): preprocessed PRSA data with hourly PM2.5, wind direction (`wd`), wind speed (`wspm`), and AQI bins (`aqi_level`).
  - Wind direction uses 16-point codes (N, NNE, …, NNW); `CV` (calm/variable) is excluded from roses.
  - AQI palette: darkgreen → gold → orange → red → purple → brown.

## How to run
```r
rmarkdown::render("Demo-3.Rmd")
```
The output is an HTML dashboard with interactive tooltips for the wind-speed bar chart.

## Interpretation tips

- NW/NE sectors tend to be cleaner (more green/yellow); SE/S/SW often show higher AQI levels (red/purple), especially in winter.
- In the bar chart, lower PM2.5 often aligns with higher mean wind speed (ventilation); higher PM2.5 with lower wind speed suggests accumulation.
