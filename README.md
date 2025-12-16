# Financial Data Science Group Project
## FIN42330 - Python for Financial Data Science

### Project Overview
This project implements a comprehensive quantitative asset allocation framework using Python to analyze and forecast excess returns for the S&P 500 and Bloomberg Barclays U.S. Aggregate Bond Index. The analysis employs rolling window estimation, OLS regression forecasting with multiple predictors, and mean-variance portfolio optimization.

### Data Sources
- **S&P 500 Index**: Monthly price data (1980-2023)
- **Bloomberg Barclays U.S. Aggregate Bond Index (LBUSTRUU)**: Monthly returns
- **Risk-Free Rate**: 3-month Treasury Bill rates
- **Predictive Variables**:
  - Dividend-to-Price Ratio (d/p)
  - Earnings-to-Price Ratio (e/p)
  - Risk-free rate (Rfree)

### Project Structure

#### 1. Data Preparation & Summary Statistics
- Import and clean monthly return data for stocks and bonds
- Calculate excess returns over the risk-free rate
- Compute comprehensive summary statistics:
  - Annualized mean returns
  - Annualized volatility
  - Sharpe ratios
  - Skewness and kurtosis

#### 2. Sample Splitting & Forecast Generation
- **In-sample period**: 1980-2000
- **Out-of-sample period**: 2001-2023
- Implements rolling window approach for dynamic forecasting
- Generates benchmark constant mean forecasts

#### 3. OLS Regression Forecasting
Implements univariate OLS models using three predictive variables:
- **d/e ratio**: Dividend-to-earnings
- **e/p ratio**: Earnings-to-price
- **Rfree**: Risk-free rate

Features:
- Rolling window estimation with expanding training sets
- Out-of-sample forecast generation
- Equal-weighted combination forecast from all predictors

#### 4. Forecast Evaluation
- **Mean Squared Forecast Error (MSFE)** calculation
- **Diebold-Mariano Test** implementation for statistical comparison of forecast accuracy
- Relative MSFE ratios comparing models to benchmark
- Statistical significance testing (p-values)

#### 5. Covariance Forecasting
- Rolling window estimation of variance-covariance matrix
- Monthly updates of 2×2 covariance matrices for portfolio construction
- Historical covariance-based risk estimation

#### 6. Portfolio Construction & Evaluation
Implements mean-variance optimization using:
- **Objective**: Maximize utility function U = E[R] - (γ/2)Var[R]
- **Risk aversion parameter** (γ): 3 (configurable: 2-5 range)
- **Dynamic weight allocation** based on expected returns and covariance forecasts
- Calculates realized portfolio excess returns

Multiple portfolio strategies evaluated:
- Benchmark (historical mean forecast)
- OLS individual predictor portfolios
- Combined forecast portfolio

Performance metrics:
- Out-of-sample portfolio returns
- Risk-adjusted performance
- Weight dynamics over time

### Key Functions

#### `sum_statistics(any_pd_series, periods)`
Calculates comprehensive summary statistics including annualized returns, volatility, Sharpe ratio, skewness, and kurtosis.

#### `split(splitter)`
Splits time series data into in-sample (1980-2000) and out-of-sample (2001-2023) periods.

#### `the_roll_window(any_series, my_splitfunction)`
Implements rolling window estimation for generating out-of-sample forecasts using historical mean.

#### `le_role_cov_forecast(any_excess_sp500, any_excess_bonds, split)`
Generates rolling covariance matrix forecasts for portfolio optimization.

#### `weights_and_portfolioreturn(expect_return, rolling_cov_forecast, realized_excess, risk_aversion=3)`
Calculates optimal portfolio weights using mean-variance optimization and computes realized portfolio returns.

#### `results_display(realized, benchmark, combo, ols_dict)`
Creates comprehensive comparison tables with MSFE metrics and Diebold-Mariano test statistics.

### Dependencies
```python
pandas
numpy
scipy.stats (norm)
matplotlib.pyplot
datetime
```

### Methodology

#### Rolling Window Approach
- Initial training window: 252 months (1980-2000)
- Window expands monthly through out-of-sample period
- Ensures no look-ahead bias in forecasting

#### Mean-Variance Optimization
Portfolio weights computed as:
```
w = (1/γ) × Σ^(-1) × μ
```
where:
- γ = risk aversion parameter
- Σ = covariance matrix forecast
- μ = expected excess return vector

#### Statistical Testing
Diebold-Mariano test compares forecast accuracy:
- H₀: No difference in predictive ability
- H₁: Alternative model has different predictive accuracy
- Uses squared forecast errors with HAC-robust standard errors

### Results Interpretation

The analysis produces:
1. **Summary statistics tables** comparing S&P 500 and Bond excess returns
2. **MSFE comparison tables** ranking forecast models by accuracy
3. **Statistical significance** of forecast improvements over benchmark
4. **Portfolio performance metrics** for different strategies
5. **Dynamic weight allocations** showing optimal asset allocation over time

### Usage

Run the Jupyter notebook cells sequentially. The analysis pipeline:
1. Loads and prepares data
2. Computes summary statistics
3. Generates forecasts using multiple methods
4. Evaluates forecast accuracy
5. Constructs optimal portfolios
6. Analyzes out-of-sample performance

### Course Context
This project demonstrates practical application of:
- Time series analysis in finance
- Predictive modeling with financial ratios
- Portfolio optimization theory
- Out-of-sample testing methodology
- Statistical evaluation of forecasting models

### Authors
Financial Data Science Master's Students
UCD Smurfit Business School

### Academic Note
This project implements concepts from empirical asset pricing and quantitative portfolio management, with emphasis on robust out-of-sample testing and statistical validation of forecasting models.
