# Analyzing Historical Stock and Revenue Data

Jupyter notebooks that download Tesla and GameStop price history, scrape their quarterly revenue tables, and plot share price next to revenue.

## Features

- Load the longest available daily history for Tesla (`TSLA`) and GameStop (`GME`) with `yfinance.Ticker.history(period="max")`, then move `Date` out of the index with `reset_index`.
- Download Tesla revenue from [revenue.htm](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/revenue.htm) and GameStop revenue from [stock.html](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/stock.html).
- Parse those HTML tables with BeautifulSoup into `Date` and `Revenue` columns, strip `$` and thousands separators, and drop empty revenue values.
- Draw a two-panel Plotly chart with `make_graph`: historical close price and historical revenue, a shared date axis, and a range slider. The function keeps stock rows on or before 14 June 2021 and revenue rows on or before 30 April 2021. The axis titles in the code are `Price ($US)` and `Revenue ($US Millions)`.

## Tech stack

The notebooks record a Jupyter kernel named `conda-env-python-py` (Python 3.7.12). They import the packages below.

| Package | Role in the notebooks | Version recorded in the files |
| --- | --- | --- |
| yfinance | `Ticker` and `history` | 0.1.67 (install cell) |
| pandas | DataFrames and `to_datetime` | 1.3.5 (saved install output, via yfinance) |
| requests | Download the revenue pages | 2.29.0 (saved install output, via yfinance) |
| beautifulsoup4 (`bs4`) | Parse revenue tables | 4.10.0 (install cell) |
| plotly | `make_subplots` and `Scatter` traces | imported; no version pin in the notebooks |
| nbformat | Installed by the first code cell | 4.2.0 |

## Project files

| File | What it contains |
| --- | --- |
| `Final Assignment(final).ipynb` | Full run. Calls `make_graph` for Tesla and GameStop. Saved outputs include both Plotly figures. Tesla revenue is cleaned with `Series.str.replace`. The stock-date cutoff written in `make_graph` is the string `'2021--06-14'`. GameStop parsing keeps rows whose date text is longer than four characters, which leaves out year-only labels. The saved `gme_revenue` table has 62 rows. |
| `Analyzing Historical StockRevenue Data and Building a Dashboard.ipynb` | The same lab with different cleaning code. `make_graph` filters stock dates with `'2021-06-14'`. Tesla revenue is stored as a list of dicts and converted with `float` before it becomes a DataFrame. The GameStop `make_graph` call is commented out. The saved Tesla graph cell raises `TypeError: Argument 'date_string' has incorrect type (expected str, got NavigableString)`. |

Both files follow the IBM Skills Network lab **Extracting and Visualizing Stock Data** (questions 1–6 in the notebook table of contents). The lab text credits Joseph Santarcangelo and Azim Hirjani, and the closing cell includes © IBM Corporation 2020.

## Setup

The first code cell of each notebook is:

```python
!pip install yfinance==0.1.67
!mamba install bs4==4.10.0 -y
!pip install nbformat==4.2.0
```

The saved output of that cell shows all three already installed in `/home/jupyterlab/conda/envs/python` (Python 3.7, with mamba pinning `python 3.7.*`). In that environment, `yfinance==0.1.67` also satisfied `pandas`, `numpy`, and `requests`. Plotly is not part of this install cell. The final notebook’s graph cells still rendered, so Plotly was already present in that environment.

On IBM Cloud Watson Studio, the notebook says to install nbformat without the `4.2.0` pin:

```python
!pip install nbformat
```

To run the notebooks on your own machine, use Jupyter and install the same pins. The `bs4` import comes from `beautifulsoup4` when `mamba` is not available. Install Plotly as well, because the notebooks import it and the install cell does not:

```bash
pip install "yfinance==0.1.67" "nbformat==4.2.0" "beautifulsoup4==4.10.0" plotly
```

The saved run resolved `yfinance==0.1.67` on Python 3.7.12. Use that Python version when a newer interpreter cannot install this pin.

## Usage

1. Start Jupyter and open `Final Assignment(final).ipynb`.
2. Run the cells from top to bottom. Later cells use `tesla_data`, `tesla_revenue`, `gme_data`, `gme_revenue`, and `make_graph` from earlier cells.
3. The saved outputs in that notebook show:
   - `tesla_data.head()` — first row dated 2010-06-29, columns `Date`, `Open`, `High`, `Low`, `Close`, `Volume`, `Dividends`, `Stock Splits`.
   - `tesla_revenue.tail()` — last rows include 2010-09-30 (31), 2010-06-30 (28), 2010-03-31 (21), 2009-09-30 (46), and 2009-06-30 (27).
   - `gme_data.head()` — first row dated 2002-02-13, with the same price columns.
   - `gme_revenue` — 62 rows; `tail()` ends at 2005-01-31 (709).
   - `make_graph(tesla_data, tesla_revenue, 'Tesla')` and `make_graph(gme_data, gme_revenue, 'GameStop')` — each saved as a Plotly figure (height 900) with share price on the top panel and revenue on the bottom panel.

`Analyzing Historical StockRevenue Data and Building a Dashboard.ipynb` leaves the GameStop chart commented out, and its saved Tesla chart cell is the `TypeError` described above.

## License

This repository is released under the MIT License. Copyright (c) 2026 Farras Inas. See [LICENSE](LICENSE).

The notebook files still carry the IBM Skills Network lab notice: © IBM Corporation 2020. All rights reserved.
