# Real-Time Market Data Pipeline

**Data Processing · Market Data · Machine Learning**

A research project covering the flow from streaming market observations to structured datasets, quantitative features, and offline machine learning experiments. This public showcase presents architecture documentation and selected configuration examples.

## Project Results

- Built an asynchronous Python WebSocket pipeline to collect Indonesian stock-market data, with recorded data covering 170 tickers.
- Processed missing order-book values and malformed records, sorted historical data by timestamp, and backtested XGBoost models with bid/ask prices and trading costs.
- Converted 14 GB of raw logs into 1.3 GB of Parquet data using out-of-core processing and Snappy compression, reducing storage requirements by approximately 90.7%.

## Problem and approach

Streaming order-book data needs to be collected, structured, and aligned in time before it can support analytical work. The project separates ingestion, transformation, feature construction, and model evaluation into distinct stages.

```mermaid
flowchart LR
    A[Market observations] --> B[Asynchronous ingestion]
    B --> C[In-memory queue]
    C --> D[Append-only storage]
    D --> E[Parsing and data quality checks]
    E --> F[Parquet datasets]
    F --> G[Market data features and labels]
    G --> H[Offline classification and backtesting]
```

## Architecture

| Stage | Technical focus |
| --- | --- |
| Ingestion | Python asynchronous I/O, queue-based separation of reception and disk writing, and timestamped records |
| Transformation | Parsing observations into a structured order-book schema, handling incomplete records, and producing Parquet datasets with Snappy compression |
| Feature calculation | Mid-price, spread, order-book imbalance, micro-price, and temporal features |
| Labeling | Time-aligned labels for a five-minute prediction horizon |
| Model evaluation | XGBoost classification and offline backtesting with bid/ask execution prices and configurable transaction costs |

## Notes

- Separating reception and persistence makes the responsibilities of each component explicit.
- Structured datasets support repeatable downstream analysis.
- Time ordering, symbol boundaries, and feature availability are essential to preventing future information from entering predictors.
- Model evaluation requires time-based validation and careful treatment of overlapping label horizons.
- Queue-based ingestion requires separate validation of capacity, shutdown behavior, and persistence guarantees.

## Technology

Python, asyncio, WebSockets, aiofiles, pandas, NumPy, Apache Parquet, Snappy, and XGBoost.

## Public materials

- `README.md`: project architecture and research scope.
- `emiten.csv`: fictional symbols illustrating the input-list format.
- `requirements.txt`: dependency reference for the ingestion component.
- `.gitignore`: exclusions for local configuration, datasets, and generated artifacts.

## About This Repository

This repository contains project documentation and selected configuration examples. The full implementation is maintained privately because parts of the code are tied to broker-specific access procedures, session handling, and private datasets.

The files shared here describe the project’s workflow and configuration without exposing account information or access methods. Any public code examples will use synthetic data or an explicitly authorized data source.

Modeling and backtesting are part of the project’s offline research. The results listed above describe data collection and processing, not validated prediction accuracy or trading profitability.
