# Project 1: Maritime Emissions Data Simulator

## Overview

This service acts as a **Data Producer** in a Producer-Consumer architecture. It simulates realistic maritime vessel emissions and operational telemetry as a continuous time-series stream.

The simulator uses a **state-based physics engine** that interpolates values from real engine test data to generate high-fidelity CO2, NOx, and fuel consumption metrics.

## High-Level Architecture

The project follows the **Producer-Consumer Pattern**:

- **Producer:** This Python service calculates engine states and broadcasts telemetry.
- **Buffer:** Data is emitted via [JSON Streaming / MQTT / FastAPI - *Choose one*].
- **Consumer:** (External Repo) Listens to the stream for visualization and analytics.

## Data Logic

Emissions are calculated using **Linear Interpolation** mapped against a reference engine profile (`data/engine_test_data.csv`).

- **Input Variable:** Engine Load % (`load_pct`)
- **Derived Metrics:** Fuel Consumption, CO2, NOx, and Exhaust Temperature.
- **Stochastic Noise:** Gaussian noise is injected into the stream to simulate real-world sensor jitter.

## Data Schema (Output)

The simulator emits JSON packets in the following format:

```json
{
  "timestamp": "ISO-8601 string",
  "vessel_id": "STRING",
  "metrics": {
    "load_pct": "FLOAT",
    "fuel_cons_kg_h": "FLOAT",
    "co2_kg_h": "FLOAT",
    "nox_kg_h": "FLOAT",
    "exhaust_temp_c": "FLOAT"
  },
  "metadata": {
    "status": "running|idle|warmup",
    "version": "1.0.0"
  }
}
```
