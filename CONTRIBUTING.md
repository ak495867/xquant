# Contributing to XQuant

First off, thank you for considering contributing to XQuant! We aim to build the most robust open-source quant platform, and your help is vital.

## Development Workflow

1.  **Fork and Clone**: Fork the repository and clone it to your local machine.
2.  **Environment Setup**: Create a virtual environment and install dependencies.
    ```bash
    python -m venv venv
    source venv/bin/activate  # or venv\Scripts\activate on Windows
    pip install -e ".[dev]"
    ```
3.  **Create a Branch**: Use descriptive names like `feature/new-indicator` or `fix/vectorized-engine-nan`.
4.  **Implement Changes**: Ensure your code follows our architectural standards.
5.  **Run Tests**: We use `pytest` for all unit and integration tests.
    ```bash
    pytest tests/
    ```
6.  **Submit PR**: Provide a clear description of your changes and why they are necessary.

## Coding Standards

- **PEP 8**: Strictly follow PEP 8 for styling.
- **Type Hinting**: All public methods and functions must have type hints.
- **Docstrings**: Use NumPy/Google style docstrings for all classes and functions.
- **Modularity**: Avoid tightly coupling components. Use the established ABCs (Abstract Base Classes) in `base.py` files.
- **Performance**: Use `pandas` and `polars` for vectorized operations. Avoid `for` loops over rows.

## Testing Requirements

- Every new feature must include a corresponding test file in `tests/`.
- Maintain or improve the overall test coverage.
- Use `unittest.mock` for any external API dependencies (yfinance, ccxt).

## Architectural Vision

XQuant is designed as a pipeline. When adding a new module:
- **Data Adapters**: Must inherit from `BaseAdapter`.
- **Features/Factors**: Must inherit from `Factor`.
- **Backtesting**: Ensure compatibility with both `VectorizedEngine` and `EventDrivenEngine` where applicable.

Happy coding!
