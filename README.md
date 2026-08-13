# safe-haven-dlm

A free-data reproduction and extension of my bachelor thesis, *The Impact of Major
Crises on Safe Haven Currencies*, written at the Chair of Economics II
(Prof. Dr. Hartmut Egger, Dr. Leandro Navarro) at the University of Bayreuth. The thesis
received a Special Prize of the Deutsche Bundesbank in 2026.

## Question

Do the safe-haven relationships of the major currencies against the US dollar shift in a
few discrete jumps, or drift continuously? The thesis used a Bai-Perron test, which assumes
discrete breaks. Here I reproduce that on free data, fix an error in the excess-return
construction, and add a state-space model that lets the coefficients drift continuously.
Neither settles the question on its own, but together they show when and how the
relationships move.

This is an in-sample structural analysis, not a forecasting exercise. That is a deliberate
choice, stated here so it is not mistaken for an omission.

## Background

The baseline is the excess-return regression of Ranaldo and Söderlind (2010): the daily
excess return of CHF, EUR, GBP and JPY against the USD, regressed on the S&P 500 return,
the change in the 10-year US Treasury yield, an FX-volatility measure, the TED spread and
the VIX. The thesis added a Bai-Perron break analysis. This repo reproduces that baseline
on free data and adds the time-varying view. The break dates are CHF 2009-03 and
2013-04, EUR 2009-01 and 2013-03, GBP 2008-10 and 2016-07, JPY 2006-06.

## Method

**Dynamic linear model (DLM).** The regression coefficients follow a random walk and
are estimated with a Kalman filter and smoother; the observation variance is constant. The
signal-to-noise ratio (lambda) is fixed rather than estimated by maximum likelihood. This
is deliberate: the maximum-likelihood estimate runs into the pile-up problem (it collapses
to zero time variation), so I fix lambda to a conservative value and show how sensitive the
paths are to it instead.

## Data

Daily levels from 2000 to 2024, all from free sources (FRED for exchange rates, yields and
interbank rates; Yahoo Finance for the S&P 500). Exchange rates are quoted as USD per unit
of foreign currency. The excess return is the daily log appreciation plus the daily
interest differential. The raw levels are committed in `data/levels.csv`, so the whole
pipeline runs from scratch. Column-by-column sources are in `data/README.md`.

## Results

All three methods agree: the safe-haven relationships shift over time. Two forces drive
this, a risk channel (equities and the flight to the dollar) and an interest-rate channel
(carry), and they behave differently in stress periods. The DLM adds a continuous-drift
view: the coefficients move most between the structural breaks, and clearly leave their
constant value only for the EUR, around the euro crisis. Because the model assumes smooth
drift, it does not settle the gradual-vs-jump question on its own; it offers the continuous
alternative next to the discrete breaks.

Central figure: the standardised time-varying coefficients with the break dates marked, in
`figures/dlm_EUR_std.png` (and the same for CHF, GBP and JPY).

Full write-up with tables, figures and interpretation: [`RESULTS.md`](RESULTS.md).

## Limitations

- Constant observation variance in the DLM. Letting it vary over time (stochastic volatility) is the natural next step; see `ROADMAP.md`.
- The random-walk state models gradual drift, not real jumps. A one-off event like the
  2015 SNB de-peg shows up as a single-day outlier, not a selected break.
- Lambda is fixed, not estimated.
- In-sample structural analysis only. No out-of-sample or predictive claim.

## Planned extensions

The natural continuation is a Bayesian extension with stochastic volatility (a
time-varying observation variance). This and other directions are outlined in `ROADMAP.md`.

## Repository structure

- `notebooks/`: 01 data, 02 baseline OLS, 03 breakpoints, 04 DLM (core), 05 diagnostics,
  06 state revisions, 07 daily-TED robustness
- `notebooks/safe_haven.py`: the shared construction the notebooks import - excess
  returns and regressors, the DLM, and the break search - so the definitions cannot
  drift apart between notebooks
- `data/`: `levels.csv` (committed raw levels) and derived outputs
- `figures/`: generated figures (300 dpi)
- `ROADMAP.md`: parked and planned work

## Reproduction

```
conda create -n dlm python=3.11
conda activate dlm
pip install -r requirements.txt
# run the notebooks in order, starting at 02 - the committed data/levels.csv
# covers the full sample, so no API key is needed
# notebook 01 only re-downloads the raw data and requires a FRED API key in
# .env (FRED_API_KEY=...); 07 uses the key too if present, else the committed
# data/ted_daily.csv
```

## Reference

Ranaldo, A. and Söderlind, P. (2010). Safe Haven Currencies. *Review of Finance*,
14(3), 385-407. https://doi.org/10.1093/rof/rfq007
