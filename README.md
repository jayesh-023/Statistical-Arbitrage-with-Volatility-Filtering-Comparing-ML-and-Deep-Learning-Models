# 📈 Statistical Arbitrage Using Supervised ML & Deep Learning on Co-Integrated Pairs

## Overview
This project implements a machine learning and deep learning-based statistical arbitrage strategy by classifying long/short trade signals on co-integrated asset pairs. It transitions from traditional regression approaches to robust supervised classification models and benchmarks against a rule-based baseline.

---

## Methodology

### 1. Pair Selection
- Identified statistically co-integrated asset pairs using the **Engle-Granger test** to ensure mean-reversion behavior.

### 2. Feature Engineering
- Main Engineered predictive features:
  - **Z-score** of spread
  - **Exponentially Weighted Moving Average (EWMA)**
  - **GARCH-based volatility estimates**

### 3. Volatility Regime Filtering
- Applied **GARCH volatility filtering** to detect high-noise or low-confidence periods, improving entry robustness.

### 4. Model Architecture & Comparison
Trained and compared multiple strategies:
-  **Baseline Rule-Based Strategy**
-  **Tree-Based Supervised ML**: Random Forest, XGBoost, LightGBM
-  **Sequence Model**: Deep RNN (LSTM)
-  **Hybrid Ensemble**: ML + LSTM
-  **Volatility-Filtered Versions** of all above

### 5. Signal Classification
- Reformulated the problem as **binary classification** (long/short) instead of regression.
- Focused on directional accuracy and confidence instead of price prediction.

### 6. Backtesting
- Simulated portfolio with **$10,000** capital.

## Evaluation Metrics
- Total Returns
- Sharpe Ratio
- Maximum Drawdown
- CAGR
- Profit Factor
- Win Rate
- Avg. Profit/Loss
- Precision  (for classification models)

---


---

## **Explainations of terminologies for beginners:**


## 🔁 **Cointegration**

- Two time series that **move independently short-term** but **stick together long-term**.
- Good for **pairs trading** → when the spread between them strays, you bet it will come back.

---

## 📉 **Regression: KO = α + β * PEP + ε**

- You model **KO based on PEP** to find a long-term relationship.
- α = intercept (fixed value)
- β = slope (how KO moves when PEP moves)
- ε = residual (spread = actual KO − predicted KO)

👉 This is a **fixed equation** once fitted — doesn’t change per time step.

Only **ε (the residuals)** change with time.

---

## 📊 **Spread**

- Difference between actual and predicted prices:
    
    ```
    
    spread = KO − (α + β * PEP)

    ```
    
- If the spread **moves around a fixed average**, it’s likely **stationary** → good for pairs trading.

---

## ⚙️ **Engle-Granger Cointegration Test**

1. Regress one series on another → get residuals (spread)
2. Run **ADF test** on residuals
3. If spread is **stationary** → the series are **cointegrated**

---

## 🧪 **ADF Test Intuition**

- Tests if a time series keeps **drifting** or **comes back to the mean**
- Equation: `Δyt = α + β * yt-1 + ...`
    - `yt` = value at time t
    - `yt-1` = value at previous time
    - `Δyt` = change in value

→ If `β` is significantly **less than 0**, the series is **stationary**

---

## ❓ **Null Hypothesis (H₀)**

- In ADF/Cointegration test:
    
    > "The series is not cointegrated (spread is not stationary)"
    > 
- If p-value < 0.05 → **Reject H₀** → Series **are cointegrated**

---

## 📏 **Z-Score**

- Measures how far a value is from the average, in terms of standard deviations:
    
    ```
    
    z = (value − mean) / std deviation
    
    ```
    
- In trading:
    - z = 0 → spread at average
    - z = ±1 → 1 std dev away
    - z = ±2 → far from mean → possible trading signal

---

## 🚨 **Standard Deviation ≠ 1**

- It’s the actual spread of your data.
- Z-score **normalizes** your value using the actual std dev.
- Empirical Rule:
    - 68% within ±1σ
    - 95% within ±2σ
    - 99.7% within ±3σ
