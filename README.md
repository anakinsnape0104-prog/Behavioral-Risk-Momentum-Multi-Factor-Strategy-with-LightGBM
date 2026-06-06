# HS300 LightGBM Multi-Factor Stock Selection Strategy

A machine learning enhanced multi-factor stock selection strategy for the CSI 300 Index universe, combining LightGBM prediction, technical factors, momentum signals, and trend-based risk control.

## Overview

This project implements a quantitative equity strategy based on the CSI 300 constituents. The strategy uses multiple technical and behavioral factors as inputs and trains a LightGBM regression model to predict future 20-day excess returns.

The final portfolio is constructed by ranking stocks according to predicted scores and selecting the top-ranked candidates while applying market trend filters and stop-loss risk management.

### Key Features

* CSI 300 constituent universe
* Multi-factor stock selection
* LightGBM regression model
* Excess return prediction framework
* Dynamic model retraining
* Trend-following market timing
* Equal-weight portfolio construction
* Individual stock stop-loss control

---

## Strategy Framework

```text
CSI300 Universe
        │
        ▼
Universe Filtering
(ST, Suspended, Newly Listed Stocks Removed)
        │
        ▼
Factor Extraction
(11 Technical & Behavioral Factors)
        │
        ▼
LightGBM Model Training
(Predict Future 20-Day Excess Return)
        │
        ▼
Score Generation
(ML Score + Factor Fallback Score)
        │
        ▼
Top 18 Stock Selection
(Equal Weight Allocation)
        │
        ▼
Market Trend Risk Control
(MA60 / MA120 Based Exposure Adjustment)
        │
        ▼
Portfolio Rebalancing
```

---

## Factors Used

The strategy incorporates 11 factors available from JoinQuant:

| Factor                    | Description                         |
| ------------------------- | ----------------------------------- |
| PSY                       | Psychological Line Indicator        |
| VR                        | Volume Ratio                        |
| DAVOL5                    | 5-Day Relative Volume               |
| VROC12                    | Volume Rate of Change               |
| ARBR                      | ARBR Sentiment Indicator            |
| Variance20                | 20-Day Volatility                   |
| sharpe_ratio_60           | 60-Day Sharpe Ratio                 |
| Skewness60                | 60-Day Return Skewness              |
| ROC20                     | 20-Day Rate of Change               |
| PLRC24                    | Price Linear Regression Coefficient |
| fifty_two_week_close_rank | 52-Week Price Ranking               |

---

## Machine Learning Model

### Model

LightGBM Regressor

### Prediction Target

Future 20-day excess return:

```text
Stock Return (t → t+20)
−
CSI300 Return (t → t+20)
```

### Training Configuration

| Parameter            | Value            |
| -------------------- | ---------------- |
| Training Window      | 120 Trading Days |
| Prediction Horizon   | 20 Trading Days  |
| Retraining Frequency | Every 5 Days     |
| Learning Rate        | 0.04             |
| Max Depth            | 6                |
| Num Leaves           | 63               |
| Boosting Rounds      | 80               |

---

## Scoring Method

The final stock score combines machine learning predictions and factor-based ranking:

```text
Final Score
=
0.85 × LightGBM Score
+
0.15 × Factor Score
```

If the model is unavailable, a factor-based fallback ranking system is used.

If factor data is unavailable, a pure momentum ranking system is applied.

---

## Portfolio Construction

### Universe

CSI 300 constituents

### Selection

Top 18 ranked stocks

### Weighting

Equal-weight allocation

### Rebalancing

Daily evaluation with periodic model retraining

---

## Risk Management

### Stop Loss

```text
Sell if loss exceeds 2%
```

### Market Trend Filter

Portfolio exposure is adjusted according to CSI300 trend conditions:

| Market Condition                  | Exposure |
| --------------------------------- | -------- |
| Above MA60                        | 100%     |
| Below MA60 but Above MA120        | 85%      |
| Below MA120                       | 65%      |
| Below MA120 and Negative Momentum | 45%      |

---

## Requirements

* JoinQuant Platform
* Python 3.x
* pandas
* numpy
* LightGBM

---

## Disclaimer

This project is for educational and research purposes only.

Past performance does not guarantee future results. Users should conduct independent research and risk assessment before deploying any quantitative strategy in live trading.

---


