# Machine Learning Enhanced Multi-Factor Equity Strategy (CSI 300)

A quantitative stock selection strategy that combines behavioral finance factors, risk factors, and momentum factors with LightGBM forecasting to construct an adaptive equity portfolio within the CSI 300 universe.

---

## Project Overview

This project develops a machine learning-enhanced multi-factor investment strategy based on constituents of the CSI 300 Index.

Instead of relying solely on traditional momentum indicators, the strategy integrates:

* Investor sentiment signals
* Risk-adjusted performance measures
* Trend-following momentum factors
* LightGBM return forecasting
* Market regime risk control

The objective is to identify stocks with superior future excess returns while maintaining robust risk management under different market environments.

---

## Research Motivation

Traditional factor investing often suffers from:

* Limited ability to model nonlinear relationships
* Information overlap among factors
* Weak adaptability across market regimes

To address these challenges, this project constructs a compact and diversified factor library and employs a LightGBM regression model to capture nonlinear interactions between factors and future stock performance.

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
Factor Construction
(Sentiment + Risk + Momentum)
        │
        ▼
Cross-Sectional Standardization
        │
        ▼
LightGBM Forecasting
(Future 20-Day Excess Return)
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
        │
        ▼
Portfolio Rebalancing
```

---

## Factor Design

The factor library follows the principle of maximizing information diversity while minimizing redundancy.

### Sentiment Factors

These factors capture investor behavior, trading enthusiasm, and short-term market sentiment.

| Factor | Description                                    |
| ------ | ---------------------------------------------- |
| PSY    | Psychological Line Indicator                   |
| VR     | Volume Ratio                                   |
| DAVOL5 | Relative Turnover Activity                     |
| VROC12 | Volume Rate of Change                          |
| ARBR   | Composite Popularity and Willingness Indicator |

### Risk Factors

These factors measure different dimensions of return uncertainty and risk-adjusted performance.

| Factor          | Description            |
| --------------- | ---------------------- |
| Variance20      | 20-Day Volatility      |
| sharpe_ratio_60 | 60-Day Sharpe Ratio    |
| Skewness60      | 60-Day Return Skewness |

### Momentum Factors

These factors capture medium- and long-term price trends.

| Factor                    | Description                     |
| ------------------------- | ------------------------------- |
| ROC20                     | 20-Day Price Momentum           |
| PLRC24                    | 24-Day Linear Trend Slope       |
| fifty_two_week_close_rank | 52-Week Relative Price Position |

---

## Machine Learning Model

### Model

LightGBM Regressor

### Prediction Target

The model predicts future 20-day excess return:

Future Stock Return − Future CSI300 Return

### Training Configuration

| Parameter            | Value                |
| -------------------- | -------------------- |
| Training Window      | 120 Trading Days     |
| Prediction Horizon   | 20 Trading Days      |
| Retraining Frequency | Every 5 Trading Days |
| Learning Rate        | 0.04                 |
| Maximum Depth        | 6                    |
| Number of Leaves     | 63                   |
| Boosting Rounds      | 80                   |

---

## Portfolio Construction

### Stock Universe

CSI 300 Constituents

### Selection Method

Stocks are ranked according to their final scores.

Final Score =

0.85 × LightGBM Prediction Score

+ 0.15 × Factor-Based Score

The highest-ranked 18 stocks are selected for portfolio construction.

### Weighting Scheme

Equal-weight allocation.

---

## Risk Management

### Individual Stock Stop-Loss

A position is liquidated when:

Loss > 2%

### Market Trend Filter

Portfolio exposure is dynamically adjusted according to the CSI300 trend.

| Market Condition                   | Exposure |
| ---------------------------------- | -------- |
| Above MA60                         | 100%     |
| Below MA60 but Above MA120         | 85%      |
| Below MA120                        | 65%      |
| Below MA120 with Negative Momentum | 45%      |

This mechanism aims to reduce drawdowns during adverse market environments.

---

## Technical Stack

* Python
* Pandas
* NumPy
* LightGBM
* JoinQuant Platform

---

## Key Contributions

* Constructed a compact Behavioral–Risk–Momentum factor framework.
* Applied machine learning to forecast future excess returns.
* Combined factor investing and nonlinear prediction techniques.
* Developed an adaptive market-regime risk control mechanism.
* Implemented a complete quantitative research and backtesting pipeline.

---

## Future Improvements

Potential extensions include:

* Feature importance analysis using SHAP
* Dynamic factor weighting
* Market state classification
* Ensemble learning models
* Northbound capital flow factors
* Fundamental and ESG factor integration

---

## Author

**Haoming Li**

Financial Engineering

Guangdong University of Foreign Studies

---

## Disclaimer

This project is intended for academic research and educational purposes only.

Past performance does not guarantee future results. All investment decisions should be made independently and at your own risk.
