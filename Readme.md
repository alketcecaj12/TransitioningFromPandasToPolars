# 🐻‍❄️ Transitioning from Pandas to Polars

> *A hands-on, notebook-driven guide for data scientists ready to level up their DataFrame game.*

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python&logoColor=white)](https://www.python.org/)
[![Polars](https://img.shields.io/badge/Polars-latest-orange?logo=polars&logoColor=white)](https://pola.rs/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)](https://jupyter.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

---

## 🧠 Why Polars?

Pandas has powered Python data science for over a decade — but the data landscape has changed. Datasets are larger, pipelines are more complex, and performance expectations are higher.

**Polars** is a blazing-fast DataFrame library written in Rust, with a clean Python API. It offers:

- ⚡ **Parallel execution** out of the box — no `.apply()` bottlenecks
- 🧮 **Lazy evaluation** via a query optimizer (think: SQL query planner for DataFrames)
- 💾 **Low memory footprint** through Apache Arrow's columnar memory model
- 🔒 **Strict, expressive type system** — fewer silent bugs
- 🧩 **Consistent, chainable API** — no index gymnastics

This repository is a **structured migration guide** — not just docs, but executable notebooks that show you exactly how to rewrite your Pandas patterns in idiomatic Polars.

---

## 📁 Repository Structure

```
TransitioningFromPandasToPolars/
│
├── 📓 CodeTransitionPandasToPolars.ipynb      # Start here: the core Pandas → Polars translation map
├── 📓 PandasDataframeVSPolarsDataframe.ipynb  # Side-by-side DataFrame comparison
├── 📓 LoadingDataInPolars.ipynb               # Reading CSV, Parquet, JSON and more
├── 📓 IndexingAndDataSelection.ipynb          # select(), filter(), slice() vs iloc/loc
├── 📓 AggregationAndGrouping.ipynb            # group_by(), agg(), and expressions
├── 📓 DataManipulationInPolars.ipynb          # with_columns(), map_elements(), expressions
├── 📓 HandlingMissingData.ipynb               # Null handling — Polars-style
├── 📓 TimeseriesAnalysisInPolars.ipynb        # Date/time operations and rolling windows
├── 📓 EfficientProcessingInPolars.ipynb       # LazyFrames and scan_* for large datasets
├── 📓 MemoryManagement.ipynb                  # Memory profiling and optimization tips
├── 📓 BenchmarkingPerformance.ipynb           # Polars vs Pandas: measured speed comparisons
├── 📓 AdvancedPolars.ipynb                    # Plugins, expressions API, and beyond
├── 📓 RealWorldDatasetExamples.ipynb          # Applied examples on real datasets
├── 📓 DebuggingTruobleshootingPolars.ipynb    # Common pitfalls and how to fix them
│
└── data/                                       # Sample datasets used in the notebooks
```

---

## 🚀 Quick-Start Example

A taste of the Polars mindset — doing the same thing in both libraries:

**Pandas**
```python
import pandas as pd

df = pd.read_csv("data/transactions.csv")
result = (
    df[df["amount"] > 1000]
    .groupby("category")["amount"]
    .mean()
    .reset_index()
    .sort_values("amount", ascending=False)
)
```

**Polars (Eager)**
```python
import polars as pl

result = (
    pl.read_csv("data/transactions.csv")
    .filter(pl.col("amount") > 1000)
    .group_by("category")
    .agg(pl.col("amount").mean())
    .sort("amount", descending=True)
)
```

**Polars (Lazy — recommended for large data)**
```python
result = (
    pl.scan_csv("data/transactions.csv")         # No data loaded yet
    .filter(pl.col("amount") > 1000)
    .group_by("category")
    .agg(pl.col("amount").mean())
    .sort("amount", descending=True)
    .collect()                                   # Execute the query plan
)
```

---

## 🗺️ Suggested Learning Path

| Step | Notebook | What You'll Learn |
|------|----------|-------------------|
| 1 | `PandasDataframeVSPolarsDataframe` | Core conceptual differences |
| 2 | `CodeTransitionPandasToPolars` | The essential translation guide |
| 3 | `LoadingDataInPolars` | I/O patterns |
| 4 | `IndexingAndDataSelection` | Selecting and filtering data |
| 5 | `AggregationAndGrouping` | Group-by and aggregation patterns |
| 6 | `DataManipulationInPolars` | Transforming columns and rows |
| 7 | `HandlingMissingData` | Null semantics |
| 8 | `TimeseriesAnalysisInPolars` | Temporal operations |
| 9 | `EfficientProcessingInPolars` | LazyFrames for scale |
| 10 | `MemoryManagement` + `BenchmarkingPerformance` | Performance tuning |
| 11 | `AdvancedPolars` + `RealWorldDatasetExamples` | Production-grade patterns |
| ⚠️ | `DebuggingTruobleshootingPolars` | When things go wrong |

---

## 🛠️ Installation

```bash
# Clone the repository
git clone https://github.com/alketcecaj12/TransitioningFromPandasToPolars.git
cd TransitioningFromPandasToPolars

# Install dependencies
pip install polars pandas jupyter pyarrow

# Launch notebooks
jupyter notebook
```

> **Python 3.9+** is recommended. Polars is actively developed — use `pip install --upgrade polars` to stay current.

---

## 🔑 Key Concepts Covered

| Concept | Pandas Equivalent | Polars Approach |
|--------|-------------------|-----------------|
| Row selection | `df.iloc[0:5]` | `df.slice(0, 5)` |
| Column filtering | `df[df['a'] > 0]` | `df.filter(pl.col('a') > 0)` |
| New column | `df['c'] = df['a'] + df['b']` | `df.with_columns((pl.col('a') + pl.col('b')).alias('c'))` |
| Group-by | `df.groupby('x')['y'].sum()` | `df.group_by('x').agg(pl.col('y').sum())` |
| Apply function | `df['x'].apply(fn)` | `df.with_columns(pl.col('x').map_elements(fn))` |
| Null check | `df.isnull()` | `df.is_null()` / `pl.col('x').is_null()` |
| Lazy mode | ❌ | `pl.scan_csv(...)...collect()` |

---

## 👤 Author

**Alket Cecaj**  
Quantitative Risk Analyst & Data Scientist | PhD | Copenhagen  
📎 [GitHub @alketcecaj12](https://github.com/alketcecaj12)

---

## ⭐ If this repo helped you think differently about DataFrames, give it a star!
