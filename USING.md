# Using XQuant-Core

This guide provides a comprehensive overview of how to use XQuant-Core for your quantitative research and trading.

## 1. Data Ingestion

The `DataLoader` is your primary entry point for fetching market data. It uses adapters to interface with different providers.

```python
from xquant-core.data.loader import DataLoader
from xquant-core.data.adapters.ccxt import CCXTAdapter

# Default yfinance loader
loader = DataLoader()
data = loader.load("AAPL", start_date="2020-01-01")

# CCXT (Crypto) loader
crypto_loader = DataLoader(adapter=CCXTAdapter({"exchange_id": "binance"}))
btc_data = crypto_loader.load("BTC/USDT", start_date="2023-01-01")
```

## 2. Feature Engineering

Use the `Factor` interface to compute technical indicators or custom microstructure features.

```python
from xquant-core.features.technical import RSI, BollingerBands

# Initialize factors
rsi_14 = RSI(window=14)
bb_20 = BollingerBands(window=20, window_dev=2)

# Compute features on your DataFrame
data['rsi'] = rsi_14.compute(data)
data['bb_width'] = bb_20.compute(data)
```

## 3. Signal Generation & Scoring

Signals are generated as a Series of target positions (-1, 0, 1). Use the `SignalScorer` to evaluate their predictive power.

```python
from xquant-core.signals.scorer import SignalScorer

# Simple signal logic
signals = (data['rsi'] < 30).astype(int)

# Calculate Information Coefficient (IC)
returns = data['close'].pct_change().shift(-1)
ic = SignalScorer.calculate_ic(signals, returns)
print(f"Signal IC: {ic:.4f}")
```

## 4. Backtesting

XQuant supports both vectorized and event-driven backtesting.

### Vectorized (Fast Research)
```python
from xquant-core.backtest.engine.vectorized import VectorizedEngine

engine = VectorizedEngine(initial_capital=100000)
results = engine.run(data, signals, commission=0.001)
```

### Event-Driven (Institutional Fidelity)
```python
from xquant-core.backtest.engine.event import EventDrivenEngine
# See xquant/backtest/engine/event.py for detailed setup
```

## 5. Performance Validation

Ensure your results aren't just the result of luck or overfitting.

```python
from xquant-core.validation.overfit import OverfittingValidator

# Calculate Deflated Sharpe Ratio
dsr = OverfittingValidator.calculate_dsr(
    sharpe=1.5, 
    trials_sharpes=[1.0, 1.2, 1.5, 0.8], 
    n_observations=1000
)
print(f"Prob. of NOT Overfit: {dsr:.4f}")
```

## 6. Reporting

Generate institutional-grade tearsheets and plots.

```python
from xquant-core.reporting.tearsheet import Tearsheet

ts = Tearsheet(results)
ts.summary()  # Prints institutional metrics
ts.generate_html("my_strategy.html")  # Exports visual report
```

## 7. Live Execution

Transition from research to production with CCXT live adapters.

```python
from xquant-core.execution.adapters.ccxt_live import CCXTLiveAdapter

config = {
    "exchange_id": "binance",
    "api_key": "YOUR_API_KEY",
    "secret": "YOUR_SECRET"
}
broker = CCXTLiveAdapter(config)
broker.authenticate()
broker.place_order("BTC/USDT", "market", "buy", 0.001)
```

---

For more details, check the source code in `xquant/` or explore the examples in `examples/`.
