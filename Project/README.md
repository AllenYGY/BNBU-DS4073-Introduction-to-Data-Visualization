# 📊 Beijing Air Quality Comprehensive Dashboard

This project builds an interactive Flexdashboard to analyze air pollution in Beijing from 2013 to 2017.

The dashboard integrates multiple analytical perspectives—Temporal Trends, Spatial Distribution, Meteorological correlations, AQI Severity, and Wind Patterns—into a unified web application. It combines Flexdashboard, Highcharter, Leaflet, Plotly, and ggplot2 to create both interactive and static visualizations.

## 📦 Prerequisites & Dependencies

To run this dashboard, ensure you have the following R packages installed:

```shell
install.packages(c(
  "flexdashboard", # Dashboard layout
  "tidyverse",     # Data manipulation (dplyr) and visualization (ggplot2)
  "lubridate",     # Date handling
  "highcharter",   # Interactive time-series charts
  "plotly",        # Interactive scatter plots and ggplot conversion
  "leaflet",       # Interactive maps
  "geosphere",     # Geographic calculations
  "GGally",        # Advanced plot extensions
  "scales",        # Formatting (percentages, etc.)
  "DT"             # Interactive data tables
))
```

## 📂 Data Preparation

### Input Data Files

The code requires two CSV files in the project root directory:

1. beijing_air_processed.csv (Main Data)

- Key Variables:
  - datetime: Date-time of observation
  - pm2_5: PM2.5 concentration (µg/m³)
  - year, hour
  - season, season_year
  - station, region
  - temp, dewp, pres: Meteorological factors
  - wd, wspm: Wind direction and speed
- Preprocessing: Missing values are handled during aggregation using na.rm = TRUE.

2. stations.csv (Geospatial Metadata)

- Key Variables:
  - station: Name matching the main dataset.
  - lat: Latitude.
  - lon: Longitude.
  - region: Categorical label (e.g., "North", "South", "Central").

### Data Transformation

Several derived variables are created using dplyr and lubridate, including daily dates, weeks, months, and seasons. These transformations enable aggregation at daily, weekly, monthly, seasonal, and yearly levels.

## 🗺️ Dashboard Sections & Visualizations

The dashboard is organized into the following tabs:

1. Overview & Trends (13 Components)

Page 1 focuses on descriptive statistics, temporal patterns, and exceedance analysis of PM2.5 using multiple time scales.

- Value Boxes (6): Summarize daily PM2.5 statistics: minimum, maximum, mean, total days, exceedance days, and exceedance rate. Implemented using flexdashboard::valueBox.
- Annual PM2.5 Trend (1): A line chart showing the annual average PM2.5 concentration, highlighting long-term trends (Highcharter).
- Seasonal PM2.5 Variation (1): Visualizes seasonal PM2.5 variation within each year (Highcharter).
- Weekly PM2.5 Cycle (1): A line chart comparing average PM2.5 across days of the week (Highcharter).
- Calendar Heatmap (1): A calendar-style heatmap displaying daily PM2.5 levels for a selected year (Highcharter heatmap with custom color scales).
- Diurnal PM2.5 Pattern (1): Shows the average PM2.5 concentration for each hour of the day (24-hour cycle) to support interactive inspection (Highcharter).
- Annual Exceedance Rate (1): A column chart presenting annual exceedance rates, with color intensity reflecting pollution severity.
- Monthly Exceedance Rate (1): Compares PM2.5 exceedance rates across months using dynamic color scales.

2. Spatial Distribution

Provides a geospatial analysis of air quality, visualizing the disparity between Northern, Southern, and Central regions.

- Interactive Leaflet Map:
  - Station Markers: Circle markers sized by mean PM2.5 concentration. Color scale: Green (Low) $\to$ Yellow (Moderate) $\to$ Red (High).
  - Regional Polygons: Uses a Convex Hull algorithm (chull) to automatically draw transparent polygons around stations belonging to specific regions (North=Blue, South=Red, Central=Orange).
  - Tooltips: Hover over any station to see the exact PM2.5 value and region.

- Statistical Analysis:
  - Distribution Boxplot: Visualizes the spread and outliers of pollution within each region.
  - Top 10 Table: A sortable table listing stations with the highest average PM2.5.
  - Regional Averages: A bar chart comparing the aggregate pollution load of the three regions.

3. Meteorological Factors

Analyzes how weather variables relate to PM2.5 using interactive scatter plots (Plotly).

- Tabset Plots:
  - PM2.5 vs Temperature
  - PM2.5 vs Dew Point
  - PM2.5 vs Pressure
  
- Features: Points colored by station, hover tooltips showing station/region/value.
- Optimization: Samples up to 3000 rows for performance.

4. Correlation Analysis

Focuses on the statistical relationships between pollution and weather.

- Correlation Heatmap: Visualizes the Pearson correlation matrix for pm2_5, temp, dewp, and pres.
- Station-wise Correlation Map (Leaflet):
  - Computes per-station correlation between PM2.5 and meteorological factors.  
  - Maps stations as colored circles by their specific cor_temp.
  - Highlights spatial heterogeneity (e.g., where temperature has a stronger cleaning or accumulating effect).

5. Wind Patterns

Visualizes directional fingerprints of pollution.

- Pollution Rose: Frequency of wind directions (16-point) colored by AQI levels.
- Winter vs Summer: Seasonal contrast of directional pollution patterns (e.g., "North cleans, South accumulates").
- Mean PM2.5 by Wind Direction: An interactive bar chart where height represents Mean PM2.5 and fill color represents Mean Wind Speed. This helps quantify how ventilation (high speed) vs. accumulation (low speed) affects specific directions.

6. AQI Analysis

Categorizes daily air quality into six severity levels (Good to Hazardous).

- AQI Classification:
  - Level 1 (Good): $\le$ 35 µg/m³
  - Level 2 (Moderate): 36-75 µg/m³
  - Level 3 to 6: Increasing severity up to Hazardous (> 250 µg/m³).
- Stacked Bar Chart: Shows the proportional distribution of AQI severity levels for each seasonal year (March–February).
- Waffle Chart: A grid-based representation where each cell represents approximately 1% of days, allowing for an intuitive "at-a-glance" comparison of pollution trends across years.

## 🚀 How to Run

1. Place the .Rmd file, beijing_air_processed.csv, and stations.csv in the same directory.
2. Run the following command in your R console or RStudio:

```r
rmarkdown::render("Demo.Rmd")
```

3. Open the generated .html file in your web browser.

## 📝 Credits

- Author:  Zhenjun LIN, Jiahao XIAO, Nengke SUN, Junya YANG, Luyao LYU.
- Date: 2025-12-18
- Tools: R, Flexdashboard, Highcharter, Plotly, Leaflet, ggplot2.
