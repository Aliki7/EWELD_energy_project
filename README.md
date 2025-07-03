# Time Series Data Preparation and Processing

## Project Overview

This project demonstrates a comprehensive time series data preparation pipeline using the publicly available EWELD (Extreme Weather Events Load Dataset). The focus is on the preparatory steps that precede building forecasting models: integration of scattered files, identification and filling of data gaps, and exploratory data analysis (EDA) for the production section (Section C). The result is a clean, consistent dataset ready for energy load forecasting applications.

## Dataset

**Source:** EWELD (Extreme Weather Events Load Dataset)  
**Reference:** J. Shao et al., "Extreme Weather Events Load Dataset (EWELD)," Scientific Data 10, 2023  
**URL:** https://www.nature.com/articles/s41597-023-02503-6  
**Data:** https://figshare.com/articles/dataset/EWELD/21893808/3?file=38873490

## Completed Tasks

1. **Data Integration**: Consolidation of multi-file EWELD dataset into CSV and Parquet formats
2. **Missing Data Identification**: Detection of missing or zero energy measurements
3. **Exploratory Data Analysis**: EDA for Section C to identify anomalies and exclude non-representative users
4. **Data Imputation**: Implementation of two methods for filling zero energy consumption values:
   - Linear interpolation
   - Time based interpolation
5. **Advanced Analysis**: In-depth stationarity analysis, decomposition, and classification for three selected consumers

## Pipeline Steps

| Step | Description |
|------|-------------|
| **Step 1: Load and verify data** | Load raw measurements, validate time labels, detect missing values and outliers |
| **Step 2: Create TimeSeries objects** | Build TimeSeries objects (one per user) with unified time axis and metadata |
| **Step 3: Stationarity analysis** | ADF and KPSS tests to assess stationarity and need for differencing |
| **Step 4: Time-series decomposition** | Additive decomposition separating long-term trend, daily rhythm, and random component |
| **Step 5: User classification** | Feature extraction and Random Forest classification to compare consumer profiles |

## Key Results

### Data Processing
- **Section C Focus**: 245 files from Manufacturing section
- **Missing Data**: 8,607,472 zeros in Energy_cons [kWh] column
- **Data Filtering**: Excluded 6 users with >70% zero values
- **Size Reduction**: ~60% compression achieved with Parquet format

### Interpolation Performance
| Method | Grouping | MAE [kWh] | RMSE [kWh] | MAPE [%] |
|--------|----------|-----------|------------|----------|
| Linear | User | 5.2 | 7.8 | 4.6 |
| Temporal | Division | 8.7 | 11.3 | 7.2 |

### Time Series Analysis
- **Stationarity**: Non-stationary in raw series; achieved stationarity after first-order differencing
- **Seasonality**: Strong daily component (lag = 96 × 15 min)
- **Decomposition**: Clear afternoon peak in daily seasonality
- **Classification**: >90% accuracy using simple features (mean, std, range, observations)

## Weather Data Integration (Preliminary)

Initial work was performed to integrate weather data with energy consumption:
- **Data Coverage**: 100% temporal overlap between datasets (2015-01-01 → 2022-11-03)
- **Extreme Events**: 7,887 rows marked as Tropical Storm events
- **Validation**: 93 events meeting wind speed criteria (≥39 mph, ≤54 mph)

*Note: Weather analysis was discontinued due to data asymmetry and quality concerns.*

## Technical Requirements

- **Python Version**: 3.10 (required for Darts library compatibility)
- **Key Libraries**: Darts, pandas, numpy, scikit-learn
- **Data Formats**: CSV, Parquet

## Project Structure

```
├── data/
│   ├── EWELD_raw/           # Original EWELD files
│   ├── processed/           # Cleaned and integrated datasets
├── notebooks/
│   ├── 01_energy_load_and_transform.ipynb
│   ├── 02_weather data merge.ipynb
│   ├── 03_explore weather data.ipynb
│   └── 04_energy_consumption_analysis.ipynb
└── README.md
```

## Conclusion

This project successfully demonstrates that careful data consolidation, precise validation of zero values, and informed selection of interpolation methods significantly improve training dataset quality. The creation of independent TimeSeries objects enabled detailed examination of seasonality and consumer variation, providing a solid foundation for further modeling approaches (e.g., SARIMAX, LSTM).

## Future Work

The prepared dataset is ready for:
- Advanced forecasting model development
- Integration with validated weather data
- Extended analysis of extreme weather impacts on energy consumption

---

*This project was completed as part of university coursework demonstrating time series data preparation methodologies.*