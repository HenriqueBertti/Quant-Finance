# data-pipeline
This repository implements a modular, end-to-end systematic trading pipeline. The architecture is designed to separate data ingestion, risk modeling, alpha generation, and portfolio optimization into distinct stages.

1. **Data Ingestion & Preprocessing (`yfinance`, `pandas`)**
   - Automatically fetches historical market data for a basket of global ETFs.
   - Applies liquidity filters (Average Daily Trading Volume) and removes assets with insufficient history to prevent look-ahead bias.
   - Calculates log returns and handles missing data.

2. **Risk Engine & Volatility Forecasting (`arch`)**
   - Implements GARCH/EGARCH models to forecast conditional volatility.
   - Establishes a "Risk Floor" to dynamically adjust exposure during high-volatility market regimes.

3. **Alpha Generation & Predictive Modeling (`TensorFlow`, `XGBoost`)**
   - Uses Machine Learning (LSTM networks and XGBoost) to predict future asset returns.
   - Integrates macroeconomic variables (e.g., VIX, 10Y Yield) and technical indicators as features.
   - Evaluates predictions using Walk-Forward Validation to ensure robustness out-of-sample.

4. **Portfolio Optimization (`scipy`, `optuna`)**
   - Applies Hierarchical Risk Parity (HRP) to cluster assets based on correlation, ensuring true diversification.
   - Utilizes stochastic winsorization to clip extreme returns and control downside risk.
   - Runs hyperparameter optimization via Optuna to fine-tune allocation weights subject to a 40% maximum concentration limit.

5. **Performance Evaluation**
   - Generates equity curves, calculates max drawdown, and outputs Sharpe ratios.
