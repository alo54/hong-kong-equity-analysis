# Hong Kong Equity Market: Data Collection and Return Analysis

A Python/Jupyter project exploring daily equity returns for constituents of the **Hang Seng Composite LargeCap & MidCap Index**. It combines a historical-price collection workflow with exploratory statistical analysis. This is a data exploration project, **not a trading strategy or backtest**.

## What I built

- Extracted Hong Kong stock codes from a Hang Seng Indexes constituent PDF dated **27 July 2026**, converting them to Yahoo Finance tickers.
- Downloaded up to five years of daily OHLCV data using `yfinance` (`auto_adjust=False`). The recorded run contains **352,201 price observations for 326 tickers**; some stocks have shorter histories.
- Standardized column names and added previous close, percentage price changes, and flags for zero/missing volume.
- Explored pooled daily percentage returns using summary statistics, a histogram, a normal Q–Q plot, and the Jarque–Bera normality test.

## Recorded results

The saved notebook output reports **351,550 non-missing daily returns**. Their mean is approximately **0.0453%**, with a median of **0%**. The maximum recorded daily change is approximately **3,646.99%**, producing extreme skewness and kurtosis. This observation has **not been validated as an economic price move** and could reflect corporate actions or data issues. The Jarque–Bera test rejects a normal distribution for the pooled returns; this does not establish a tradable signal.

## Files

| File | Purpose |
| --- | --- |
| `get_data.ipynb` | Extract constituents, download prices, inspect data, and export a standardized CSV. |
| `stats.ipynb` | Load the exported CSV and explore the distribution of daily returns. |
| `Constituents_ENG_02014.pdf` | Constituent source used by the collection notebook. |
| Other supplied PDF/XLS files | Reference material from the original research folder; not required by the executed notebook code shown here. |

## Reproducing the analysis

1. Use Python 3.10+ and install the dependencies:

   ```bash
   pip install -r requirements.txt
   ```

2. Open `get_data.ipynb` from the repository root, with `Constituents_ENG_02014.pdf` in that same directory. Run its cells in order. The download step requires internet access and may take time. It writes `hk_market_5y.parquet` and, later, `hk_market_5y.csv`.
3. Run `stats.ipynb` **after** generating `hk_market_5y.csv` in the same working directory.

The notebooks currently use **relative paths based on the working directory**, so running them from a different directory may require path changes. Price history and the constituent universe are time-sensitive: a new download may not reproduce the saved outputs exactly.

## Methodological limitations

- **Survivorship bias:** constituents are selected as of 27 July 2026, then traced backward. Stocks that previously left the index are excluded.
- **Unadjusted prices:** the downloaded series uses `auto_adjust=False`, and percentage changes are calculated from `Close`, not `Adj Close`. Splits, dividends, and other corporate actions may distort return calculations.
- **Pooled observations:** the histogram and Jarque–Bera test combine returns from many stocks and dates; these observations are not independent and identically distributed.
- **Extreme outliers:** unusually large price changes require validation against the original source and corporate-action records.
- **ADF cell:** the original statistical notebook calls `adfuller` on the pooled return vector. This is not a meaningful chronological series for a single asset; **do not interpret that output as a valid stationarity conclusion**.
- **Volume flags:** zero or missing volume is used as a proxy for suspension, not a verified exchange halt.
- **Schema:** the current code does not create a `turnover` column or a `turnover_rate` column. Some China-specific fields are set to missing/false for compatibility rather than inferred from HK market data.

## Data provenance

Constituents: Hang Seng Indexes constituent report (27 July 2026). Historical OHLCV: Yahoo Finance through `yfinance`. Data is intended for educational and exploratory analysis, not production trading.
