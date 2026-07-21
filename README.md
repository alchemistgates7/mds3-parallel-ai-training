# MDS3 Parallel AI Model Training

This repository contains the implementation and results for the MDS3 Parallel Computing assignment:

**Parallel AI Model Training and Hyperparameter Search**

## Project Objective

The project investigates whether parallel model evaluation in Apache Spark can reduce the total time required for hyperparameter tuning.

The machine-learning task predicts whether a stock's closing price will increase on the next trading day.

## Dataset

A historical stock-price dataset downloaded from Kaggle was used.

The raw dataset is not included in this repository. To run the notebook:

1. Download the stock dataset from Kaggle.
2. Upload the CSV file to Google Drive or Colab.
3. Update the `data_path` variable in the notebook.

Expected source columns include:

- Date
- Ticker
- Open
- High
- Low
- Close
- Volume

## Feature Engineering

The model uses seven historical market features:

- Daily return
- Five-day return
- Five-day moving-average ratio
- Twenty-day moving-average ratio
- Ten-day volatility
- Volume change
- Intraday price range

The stock ticker is encoded using `StringIndexer` and `OneHotEncoder`.

## Model

The experiment uses PySpark MLlib's `RandomForestClassifier`.

| Configuration | numTrees | maxDepth |
|---|---:|---:|
| E1 | 20 | 5 |
| E2 | 20 | 10 |
| E3 | 50 | 5 |
| E4 | 50 | 10 |

## Parallelism Experiment

The following tuning parallelism levels were compared:

- `parallelism=1`
- `parallelism=2`
- `parallelism=4`

Each experiment was executed twice. Median runtime was used for comparison.

| Parallelism | Median Time | Speedup |
|---:|---:|---:|
| 1 | 36.58 s | 1.00x |
| 2 | 31.80 s | 1.15x |
| 4 | 31.92 s | 1.15x |

The Colab environment provided two CPU cores. Increasing parallelism from 1 to 2 reduced runtime, while increasing it to 4 produced no meaningful additional improvement.

## Final Model

The selected configuration was:

- `numTrees=20`
- `maxDepth=10`

The tuned model improved test F1-score from approximately `0.4392` to `0.4700`.

## Repository Structure

```text
.
├── MDS3_Parallel_AI_Training.ipynb
├── README.md
├── results/
│   ├── benchmark_raw_results.csv
│   ├── configuration_results.csv
│   ├── parallelism_timing_results.csv
│   ├── configuration_f1_comparison.png
│   └── parallelism_time_comparison.png
└── report/
    └── MDS3_Parallel_AI_Training_Report.pdf
