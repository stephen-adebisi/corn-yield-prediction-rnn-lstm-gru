# 🌽 Predicting Corn Yield from Satellite and Weather Time Series
### A Comparison of RNN, LSTM, and GRU

Comparing three recurrent neural network architectures for annual county-level corn yield estimation in South Dakota.

🛰️ Remote Sensing · 🌦️ Weather Data · 🧠 Deep Learning · 🐍 Python

## 🎯 Project Objective

How accurately can Simple RNN, LSTM, and GRU estimate annual corn yield using within-year satellite and weather sequences?

This project prepares sequential inputs, trains all three models, and evaluates their predictions on held-out years.

## 📊 Dataset

| Attribute | Description |
|---|---|
| Study area | South Dakota, USA |
| Period | 2006–2020 |
| Records | 751 county-year observations |
| Counties | 66 |
| Input sequence | 11 time steps × 10 variables |
| Target | Annual county-level corn yield |

**Satellite predictors:** GPP, NDVI, and EVI.

**Weather predictors:** precipitation, mean/minimum/maximum temperature, mean dew-point temperature, and minimum/maximum vapor pressure deficit.

County identifiers and years are retained for tracking records and splitting the dataset. They are not model predictors.

The exact dates of the 11 observation periods and the source units require confirmation from the dataset documentation.

## ⚙️ Workflow

1. Load the Excel dataset and check data quality.
2. Arrange predictors into sequences with all 10 variables aligned at each time step.
3. Split the dataset chronologically.
4. Fit Min–Max scalers using training data only.
5. Train Simple RNN, LSTM, and GRU models.
6. Evaluate predictions in the original yield scale.
7. Visualize learning curves and observed versus predicted yields.
8. Export test metrics and predictions.

### 🗓️ Temporal Split

| Set | Years | Purpose |
|---|---|---|
| Training | 2006–2016 | Learn model parameters |
| Validation | 2017–2018 | Monitor training and select best weights |
| Testing | 2019–2020 | Evaluate held-out performance |

### 🧠 Model Setup

Each model uses:

- Two recurrent layers with 64 and 32 units
- Dropout of 0.2 after each recurrent layer
- A linear output layer
- Adam optimizer with an initial learning rate of 0.001
- Mean squared error loss
- A maximum of 200 epochs and batch size of 32
- Early stopping and learning-rate reduction
- Random seed: 42

Layer sizes are shared across models, although their parameter counts differ.

## 🏆 Test Results

Performance on the held-out 2019–2020 records:

| Model | MAE ↓ | RMSE ↓ | R² ↑ |
|---|---:|---:|---:|
| **LSTM** | 15.700 | **18.279** | **0.699** |
| **GRU** | **15.682** | 18.392 | 0.696 |
| Simple RNN | 16.826 | 23.086 | 0.521 |

MAE and RMSE are reported in the dataset’s original yield units.

### 💡 Findings

- LSTM achieved the lowest RMSE and highest R².
- GRU performed almost identically and achieved the lowest MAE.
- Both gated architectures outperformed Simple RNN in this run.
- Test performance was lower than validation performance for all three models.

The small difference between LSTM and GRU does not establish a clear advantage for either architecture.


**Main dependencies:** TensorFlow, NumPy, pandas, scikit-learn, Matplotlib, and openpyxl.


## 📌 Limitations

- Results reflect one random seed and one temporal split.
- No statistical significance test was performed between models.
- Counties can appear in both training and testing years; this is a temporal evaluation, not a test of transfer to unseen counties.
- The project estimates annual yield from within-year observations. Early-season forecasting requires a defined observation cutoff.
- Dataset provenance, observation dates, scale factors, yield units, and redistribution terms need to be documented fully.

## 📚 Research Background and Acknowledgments

The project was informed by:

- Khan, Li, and Maimaitijiang (2022), *A Geographically Weighted Random Forest Approach to Predict Corn Yield in the US Corn Belt*.  
  https://doi.org/10.3390/rs14122843

- Khan, Li, and Maimaitijiang (2024), *Using Gross Primary Production Data and Deep Transfer Learning for Crop Yield Prediction in the US Corn Belt*.  
  https://doi.org/10.1016/j.jag.2024.103965
