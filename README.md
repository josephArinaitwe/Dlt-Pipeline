# OpenLibrary dlt Pipeline

This repository demonstrates a beginner-friendly `dlt` pipeline that pulls book data from the Open Library API and loads it into DuckDB.

## What this project does

- Extracts data from `https://openlibrary.org/search.json`
- Uses a default query of `"harry potter"` (configurable in the source function)
- Normalizes and loads data with `dlt`
- Writes data to a DuckDB destination under dataset `ol_data`

The pipeline is configured with:

- `pipeline_name="ol_demo"`
- `destination="duckdb"`
- `dataset_name="ol_data"`

## Repository structure

- `dlt_pipeline.ipynb`: Main notebook to build and run the pipeline
- `dlt_pipeline_overview.ipynb`: Step-by-step notebook showing extract/normalize/load and inspection
- `.dlt/secrets.toml`: `dlt` destination configuration (local secrets file)

## Prerequisites

- Python 3.9+
- `pip`
- Jupyter Notebook or JupyterLab

## Setup

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install "dlt[duckdb]" notebook
```

## Configure dlt secrets

The repo already includes:

```toml
[destination.duckdb.credentials]
```

You can optionally set a database file explicitly:

```toml
[destination.duckdb.credentials]
database = "openlibrary.duckdb"
```

## Run the pipeline

1. Start Jupyter:
   ```powershell
   jupyter notebook
   ```
2. Open `dlt_pipeline.ipynb` (or `dlt_pipeline_overview.ipynb`).
3. Run cells from top to bottom.

The notebooks define `openlibrary_source(...)`, create the `dlt` pipeline, and run extraction/loading.

## Inspect loaded data

In the overview notebook, you can inspect results with:

```python
ds = pipeline.dataset()
ds.tables
df = ds.books.df()
df.head()
```

## Notes

- The notebooks include environment variable setup to force `dlt` to read `.dlt/secrets.toml`, which is useful in notebook environments like Colab.
- Do not commit real credentials or sensitive values to `.dlt/secrets.toml`.
