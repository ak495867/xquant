# XQuant-Core

**Institutional-Grade Quantitative Trading Framework for Alpha Research and Production**

XQuant is a high-performance, modular framework designed for the development, backtesting, and deployment of quantitative trading strategies. Built with an emphasis on mathematical rigor and architectural flexibility, XQuant provides researchers and engineers with the tools necessary to bridge the gap between abstract alpha discovery and systematic production execution.

---

## Technical Foundations

### Core Architecture
XQuant utilizes a strictly decoupled, layered architecture to ensure modularity and extensibility:

*   **Data Orchestration**: Standardized OHLCV ingestion with support for Equities, Digital Assets, and FX via optimized adapters (yfinance, CCXT, OANDA). Features include multi-asset alignment, timezone normalization, and adaptive trading calendars.
*   **Feature Engineering**: A comprehensive library of 50+ technical indicators and microstructure metrics (OFI, VWAP Deviation, Bid-Ask Spread). Supports custom factor development via an abstract Factor API.
*   **Signal Discovery**: Institutional-grade signal evaluation using Information Coefficient (IC), ICIR, and Rank IC. Includes advanced signal decay and half-life analysis.
*   **Dual-Engine Backtesting**:
    *   **Vectorized**: Ultra-fast NumPy/Pandas engine for high-throughput parameter optimization.
    *   **Event-Driven**: High-fidelity simulation utilizing an asynchronous event loop for realistic execution modeling, slippage, and market impact.
*   **Statistical Validation**: Robustness suite featuring Walk-Forward Optimization (WFO), Monte Carlo simulations, and Overfit Detection using the Deflated Sharpe Ratio (DSR).
*   **Risk & Portfolio Management**: Advanced metrics suite (Sharpe, Sortino, VaR, CVaR) coupled with volatility-targeted position sizing and Kelly Criterion allocation.
*   **Live Execution Infrastructure**: Low-latency broker adapters with support for paper trading and live production hooks (CCXT).

---

## System Components

| Layer | Functional Scope |
| :--- | :--- |
| **Ingestion** | Multi-source adapters, OHLCV normalization, resampling. |
| **Features** | Technical indicators, microstructure, cross-asset correlations, regime detection. |
| **Signals** | Alpha discovery, scoring, ensemble combination, decay analysis. |
| **Backtest** | Vectorized research engine, Event-driven execution engine. |
| **Validation** | OOS testing, WFO, Monte Carlo, Bias/Overfit detection. |
| **Risk** | Institutional performance metrics, exposure tracking, drawdown analysis. |
| **Execution** | Paper trading simulation, Live broker adapters. |
| **Analytics** | Performance tearsheets, high-fidelity visualization, data export. |

---

## Installation and Deployment

### Prerequisites
- Python 3.9+
- NumPy, Pandas, Polars
- CCXT (for digital asset execution)

### Setup
```bash
git clone https://github.com/ak495867/xquant.git
cd xquant
pip install -e .
```

---

## Usage Overview

### Automated Strategy Execution
XQuant provides comprehensive examples to demonstrate full-pipeline execution:

```bash
# Execute Equity Momentum Strategy
python examples/equity_momentum.py

# Execute Crypto Mean Reversion Strategy
python examples/crypto_mean_reversion.py
```

### Strategic Research Workflow
Researchers can utilize the modular API to build custom pipelines:

```python
from xquant.data.loader import DataLoader
from xquant.features.technical import RSI
from xquant.backtest.engine.vectorized import VectorizedEngine

# Ingestion
data = DataLoader().load("AAPL", start_date="2022-01-01")

# Engineering
data['rsi'] = RSI(window=14).compute(data)

# Signal and Backtest
signals = (data['rsi'] < 30).astype(int)
results = VectorizedEngine().run(data, signals)
```

---

## Development and Contributions

XQuant is built for the community. We maintain rigorous standards for code quality and mathematical accuracy. Please refer to [CONTRIBUTING.md](CONTRIBUTING.md) for technical specifications on adding new features or adapters.

## License

XQuant is distributed under the **MIT License**.
