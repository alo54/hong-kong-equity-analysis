# Hong Kong Market Dataset (Hang Seng Composite LargeCap & MidCap)

## Overview

This dataset contains daily historical market data for the constituents of the **Hang Seng Composite LargeCap & MidCap Index**.

The constituent list was obtained from the official Hang Seng Indexes constituent report dated **27-07-2026**. Historical daily price data were downloaded from **Yahoo Finance** using the `yfinance` Python package.

The dataset is intended for quantitative research, factor analysis, and algorithmic trading experiments.

---

## Universe

- **Index:** Hang Seng Composite LargeCap & MidCap Index
- **Number of stocks:** 326
- **Exchange:** Hong Kong Stock Exchange (HKEX)
- **Market:** Hong Kong
- **Frequency:** Daily
- **History:** Last 5 years

---

## Data Source

### Constituents

Official Hang Seng Indexes constituent report.

### Historical prices

Yahoo Finance (`yfinance`)

---

## Columns

| Column | Type | Description |
|---------|------|-------------|
| date | datetime | Trading date |
| ticker | string | Yahoo Finance ticker (e.g. `0700.HK`) |
| market | string | Market identifier (`HK`) |
| exchange | string | Exchange (`HKEX`) |
| board | string | Trading board (`Main Board`) |
| open | float | Opening price |
| high | float | Highest traded price |
| low | float | Lowest traded price |
| close | float | Closing price |
| volume | integer | Number of shares traded |
| turnover | float | Approximate trading value (`close × volume`) |
| turnover_rate | float | Not available from Yahoo Finance. Stored as `NaN`. |
| pct_change | float | Daily percentage return computed from previous close |
| prev_close | float | Previous trading day's closing price |
| is_st | boolean | Always `False`. ST designation exists only in Mainland China. |
| limit_pct | float | Not applicable to HK market. Stored as `NaN`. |
| limit_up_price | float | Not applicable to HK market. Stored as `NaN`. |
| limit_down_price | float | Not applicable to HK market. Stored as `NaN`. |
| is_limit_up | boolean | Always `False`. |
| is_limit_down | boolean | Always `False`. |
| is_suspended | boolean | `True` if trading volume is zero or missing. |
| is_zero_or_missing_volume | boolean | `True` if trading volume is zero or missing. |
| is_suspended_or_halted | boolean | Same as `is_suspended`. |

---

## Derived Variables

### Previous Close

```
prev_close(t) = close(t-1)
```

Computed independently for each ticker.

---

### Daily Percentage Change

```
pct_change =
(close - prev_close)
/ prev_close
× 100
```

Expressed as a percentage.

---

### Turnover

```
turnover = close × volume
```

This is an approximation of the daily trading value.

It should **not** be interpreted as turnover rate.

---

## Missing Fields

Yahoo Finance does not provide several fields available in Mainland Chinese market data providers (e.g. Baostock).

The following variables are therefore unavailable:

- turnover_rate
- limit_pct
- limit_up_price
- limit_down_price

These columns are included for schema compatibility and contain missing values (`NaN`).

---

## Notes

This dataset is **not survivor-bias free**.

The constituent universe corresponds to the Hang Seng Composite LargeCap & MidCap Index constituents on **27 July 2026**, while historical prices extend approximately five years into the past. Companies that left the index before this date are not included.

---

## Intended Uses

- Factor research
- Portfolio construction
- Risk analysis
- Statistical analysis
- Backtesting
- Machine learning for financial markets

---

## Disclaimer

Historical prices were downloaded from Yahoo Finance through the `yfinance` library. While suitable for research and educational purposes, the dataset should not be considered an official market data source for production trading systems.