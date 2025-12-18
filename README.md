# DataVisualization Coursework & Project

This repo hosts course labs (R basics to dashboards) and a capstone project on Beijing air quality (PRSA) with preprocessing and multiple flexdashboards.

## Structure

- **Project/**: End-to-end preprocessing (`data_preprocess.Rmd`) and dashboards (`Demo.Rmd`, `wind_analysis.Rmd`) built on `beijing_air_processed.csv` and `stations.csv`.
- **Lab-Exercise/**: Lab-1 … Lab-10 folders (R basics, EDA, ggplot2, multi/bivariate graphs, interactivity, dashboards). See `Lab-10 - dashboard` for flexdashboard examples.
- **Lab-Ans/**: Lab answer key/reference.
- **Assignment/**: `As-1 - R Tutorial Exercise`, `As-2`.

## Getting started

1) Open `DataVisualization.Rproj` in RStudio.
2) Install dependencies used across labs and project:
   ```r
   install.packages(c(
     "tidyverse","lubridate","flexdashboard","plotly","leaflet",
     "highcharter","DT","scales","viridis","janitor","zoo","slider"
   ))
   ```
3) For labs: open the target `Lab-Exercise/Lab-*` Rmd/R files and knit/run as guided in each folder; `Lab-Ans` holds references.
4) For the project dashboards:
   ```r
   setwd("Project")
   rmarkdown::render("Demo-1.Rmd")  # KPIs & trends
   rmarkdown::render("Demo-2.Rmd")  # Spatial map & meteo
   rmarkdown::render("Demo-3.Rmd")  # Wind mechanisms
   rmarkdown::render("Demo.Rmd")    # Combined dashboard
   ```
   To rerun preprocessing: `rmarkdown::render("data_preprocess.Rmd", params = list(write_output = TRUE))`.

## Project data notes

- `beijing_air_processed.csv`: preprocessed PRSA hourly data with seasonal year (Mar–Feb), AQI bins, simplified wind, region tags, cumulative wind/rain.
- `stations.csv`: station coordinates/region for mapping.
- Raw inputs: `PRSA_Data/` CSVs (already ingested by `data_preprocess.Rmd` when needed).

## Tips

- Knit dashboards from the `Project/` directory to ensure relative paths resolve.
- If ggplotly/leaflet output is slow, reduce sample sizes or close other heavy graphics windows.
