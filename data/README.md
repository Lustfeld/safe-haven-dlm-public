# Data

`levels.csv` holds daily levels from 2000-01-01 to 2024-06-30 - a free-data reproduction
of the safe-haven dataset (originally Bloomberg / Refinitiv).

## Columns

| Column   | Description                        | Source                      |
| -------- | ---------------------------------- | --------------------------- |
| EUR      | USD per euro                       | FRED DEXUSEU                |
| GBP      | USD per pound                      | FRED DEXUSUK                |
| JPY      | USD per yen                        | FRED DEXJPUS (reciprocated) |
| CHF      | USD per franc                      | FRED DEXSZUS (reciprocated) |
| SP500    | S&P 500 index level                | Yahoo Finance ^GSPC         |
| VIX      | CBOE volatility index              | FRED VIXCLS                 |
| UST10Y   | 10-year Treasury yield (% p.a.)    | FRED DGS10                  |
| TBILL_3M | 3-month Treasury bill (% p.a.)     | FRED DTB3                   |
| US_3M    | US 3-month interbank rate (% p.a.) | FRED IR3TIB01USM156N        |
| EUR_3M   | Euro-area 3-month interbank rate   | FRED IR3TIB01EZM156N        |
| GBP_3M   | UK 3-month interbank rate          | FRED IR3TIB01GBM156N        |
| JPY_3M   | Japan 3-month interbank rate       | FRED IR3TIB01JPM156N        |
| CHF_3M   | Switzerland 3-month interbank rate | FRED IR3TIB01CHM156N        |

## Notes

- Exchange rates are quoted as USD per unit of foreign currency; JPY and CHF are
  reciprocated from their native FRED quotes.
- Interbank rates are OECD monthly series, forward-filled to daily frequency.
- The 10-year term uses the Treasury yield (DGS10), not the futures price used in
  the thesis; the notebooks use the daily change of this yield (percentage points)
  as the regressor.
- Japan's interbank series starts in 2002, so JPY_3M has fewer observations.

_Downloaded: 2026-08-13_
