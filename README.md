# Gradient Boosting vs. Croston-Type Models for Intermittent Spare-Parts Demand Forecasting

Master's thesis project — Digital Management (M.A.), Hochschule Fresenius.

## What this project does

Compares seven forecasting models on simulated aviation spare-parts demand data (500 SKUs, 313 weeks):

- **Classical methods:** Croston, SBA, TSB, Seasonal Naive
- **ML methods:** XGBoost, LightGBM, Random Forest

Evaluation covers both forecast accuracy (MAE, RMSE, sMAPE, MASE) and inventory performance (fill rate, stockout rate, holding cost) under a periodic-review (s, S) policy.

## Key results

| Model | MAE | Fill Rate | Holding Cost (EUR/wk) |
|-------|-----|-----------|----------------------|
| XGBoost | 0.124 | 86.0% | 7,544 |
| LightGBM | 0.139 | 86.1% | 8,017 |
| Random Forest | 0.556 | 90.7% | 6,831 |
| TSB | 1.563 | 94.5% | 7,207 |
| SBA | 1.670 | 95.0% | 7,085 |
| Croston | 1.695 | 95.3% | 7,273 |
| Seasonal Naive | 2.029 | 98.1% | 11,920 |

ML models achieve large accuracy gains but lower fill rates due to reduced forecast bias. Safety stock recalibration is needed when switching from classical to ML-based forecasting.

**Note:** These results are from simulated data. The ~90% MAE reduction reflects the combined effect of global modelling, feature engineering, fixed classical smoothing parameters, and controlled simulation conditions. Real-world improvements would likely be smaller.

## Repository structure

```
├── thesis_analysis.ipynb              # Main notebook (data generation, modelling, evaluation)
├── thesis_analysis_executed.ipynb     # Notebook with saved outputs
├── aviation_spare_parts_demand_dataset.csv   # Simulated weekly demand (500 parts x 313 weeks)
├── aviation_spare_parts_complete.xlsx        # Complete dataset in Excel
├── part_master_data.csv                      # Part attributes (type, criticality, MTBF, cost)
├── part_demand_statistics.csv                # Per-part demand summary statistics
├── structure.md                              # Thesis chapter structure
├── figures/                                  # All generated figures and result tables
│   ├── fig_1_1_intermittent_demand_example.png
│   ├── fig_5_4_model_comparison.png
│   ├── thesis_results_all.xlsx               # Raw results (7 sheets)
│   └── ... (17 figures total)
└── README.md
```

## How to run

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost lightgbm scipy openpyxl
jupyter notebook thesis_analysis.ipynb
```

Tested on Python 3.10. No GPU required. Full pipeline runs in under 10 minutes on standard hardware.

## Dataset

The dataset is simulated, not sourced from an operational airline. It was calibrated against published aviation MRO demand parameters:

- 72.3% zero-demand rate (within the 60-80% range from Regattieri et al., 2021)
- Syntetos-Boylan category distribution aligned with Babai et al. (2020)
- Three part types: expendable, repairable, rotable
- Three criticality classes: AOG-critical, essential, routine

## Feature engineering

32 features across six categories:

- **Lag features (6):** demand at lags 1, 2, 3, 4, 8, 12 weeks
- **Rolling statistics (12):** mean, std, max over 4/8/13/26-week windows
- **Calendar (3):** week, month, quarter
- **Intermittent-specific (2):** weeks since last demand, non-zero count in 13 weeks
- **Part attributes (5):** type, criticality, aircraft type, unit cost, lead time
- **Operational (4):** MTBF, cumulative demand, average order quantity, demand CV

## Citation

If you use this code or dataset, please cite:

```
Veera Sai Prakash (2026). Gradient Boosting vs. Croston-Type Models for Forecasting
Intermittent Spare-Parts Demand: Evidence from Publicly Available Aviation
Maintenance Data. Master's thesis, Hochschule Fresenius.
```

## License

MIT
