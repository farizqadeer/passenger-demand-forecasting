# MRT-3 Passenger Demand Forecasting & Overcrowding Detection

A deep learning project that predicts hourly passenger demand for MRT-3 (Metro Rail Transit Line 3, Manila) and detects overcrowding for the next 24 hours using a BiLSTM model.

---

## What This Project Does

- Loads and cleans MRT-3 ridership data (2023–2025)
- Augments the data to improve model training
- Engineers time-based features like peak hours, lag values, and cyclical encoding
- Trains a Bidirectional LSTM (BiLSTM) model to forecast the next 24 hours
- Classifies each hour as Normal, Moderate, Critical, or System Closed
- Gives operational suggestions like deploying 4-car trains or reducing headway

---

## Files You Need Before Running

| File | Where It Comes From |
|------|-------------------|
| `MRT3_LSTM_FINAL_v3.ipynb` | This notebook |
| `2023-25_Ridership__Northbound-Southbound_Mr__Aldrich_Orsolino__1_.xlsx` | Your ridership dataset |

Both files need to be uploaded to Google Colab before running.

---

## How to Run

1. Go to [colab.research.google.com](https://colab.research.google.com)
2. Upload `MRT3_LSTM_FINAL_v3.ipynb`
3. Once open, upload the Excel ridership file using the Colab file panel on the left sidebar
4. Click **Runtime → Run All**
5. Wait for all cells to finish — takes around 3 to 5 minutes

That is it. Everything runs automatically from top to bottom.

---

## Notebook Walkthrough

| Step | What Happens |
|------|-------------|
| Step 0 | Installs and imports all libraries |
| Step 1 | Loads the Excel file and the platform capacity table |
| Step 2 | Cleans the data — removes non-operating hours, fixes types, computes daily averages |
| Step 2B | Augments the data from 54 rows to 3,240 rows using realistic daily variation |
| Step 3 | Creates features — peak flags, cyclical hour encoding, lag values, scaling |
| Step 4 | Builds input sequences for the LSTM (5 past hours → predict 24 future hours) |
| Step 5 | Trains the BiLSTM model with early stopping |
| Step 6 | Evaluates the model — MAE, RMSE, SMAPE on 2025 test data |
| Step 7 | Generates the 24-hour forecast starting from 05:00 |
| Step 8 | Classifies each hour — Normal, Moderate, Critical, or System Closed |
| Step 9 | Prints the full operational report with suggestions per hour |
| Step 10 | Saves all output files |

---

## Output Files

After running, these files are saved automatically:

| File | Description |
|------|-------------|
| `mrt3_cleaned_ridership.csv` | Cleaned dataset |
| `mrt3_bilstm_v3_model.keras` | Trained model |
| `feature_scaler.pkl` | Feature scaler |
| `target_scaler.pkl` | Target scaler |
| `mrt3_model_evaluation.csv` | MAE, RMSE, SMAPE results |
| `mrt3_24h_forecast_results.csv` | 24-hour predictions with overcrowding status and suggestions |
| `step5_training_curves.png` | Training loss chart |
| `step6_actual_vs_predicted.png` | Actual vs predicted chart |
| `step8_overcrowding_forecast.png` | 24-hour overcrowding bar chart |

---

## Model Results

| Metric | Value |
|--------|-------|
| MAE (all hours) | 2,055 passengers/day |
| MAE (peak hours) | 2,001 passengers/day |
| RMSE | 2,569 passengers/day |
| SMAPE | 13.47% |

---

## Overcrowding Levels

| Level | Condition | What It Means |
|-------|-----------|---------------|
| 🟢 Normal | Below 65% capacity | All good, standard operations |
| 🟡 Moderate | 65% – 90% capacity | Getting busy, deploy 4-car trains |
| 🔴 Critical | Above 90% capacity | Overcrowded, reduce headway immediately |
| ⚫ System Closed | 00:00 – 04:00 | MRT-3 not operating |

Capacity is based on official DOTr-MRT3 figures — 394 passengers per car, system throughput of 23,640 passengers per hour during peak.

---

## Requirements

No local installation needed. Just Google Colab.

If running locally:

```
pip install pandas numpy matplotlib seaborn scikit-learn tensorflow openpyxl
```

Python 3.10 or higher recommended.

---

## Data Sources

- MRT-3 Ridership Data — provided by Mr. Aldrich Orsolino (DOTr-MRT3, 2023–2025)
- Platform Capacity — FOI Response, DOTr-MRT3, General Manager Michael J. Capati, March 2026
- Train Schedule and Fleet Info — dotrmrt3.gov.ph
