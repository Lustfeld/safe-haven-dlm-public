# safe-haven-dlm

Free-data reproduction and extension of a bachelor thesis on safe-haven currencies
(CHF, EUR, GBP, JPY vs. USD). In-sample structural analysis — not a forecasting
project. See `README.md` for the question/method, `RESULTS.md` for the full
write-up with tables and figures, `ROADMAP.md` for parked/planned work.

## Structure

- `notebooks/01`–`09`: ordered pipeline, each depends on outputs of the previous
  ones. Run in order when reproducing from scratch.
  - `01` data, `02` baseline OLS, `03` Bai-Perron breakpoints, `04` DLM (core
    result), `05` diagnostics, `06`/`07` stochastic-volatility extension (MCMC,
    slow), `08` state revisions, `09` daily-TED robustness check.
- `data/levels.csv`: committed raw daily levels — the pipeline runs from scratch
  without needing to re-download. Re-downloading (notebooks 01, 09) needs
  `FRED_API_KEY` in `.env`. Other `data/*.csv` are generated outputs.
- `figures/`: generated at 300 dpi; regenerating overwrites in place.
- `RESULTS.md` is the source of truth for interpretation/numbers — if a
  notebook's output would change a stated result or figure, flag it rather than
  silently letting the two drift apart.

## Working conventions

- Notebooks 06 and 07 run MCMC (PyMC/NUTS) and are slow. Don't re-run them
  speculatively; ask first if a change would require it.
- The DLM's signal-to-noise ratio (lambda) is deliberately fixed, not
  MLE-estimated (pile-up problem — see README "Method" and ROADMAP item 2).
  Don't switch this to MLE estimation without flagging the tradeoff.
- No out-of-sample/predictive claims anywhere in this project by design (see
  README "Limitations"). Don't frame results in forecasting terms.
- Python 3.11, deps in `requirements.txt`; env setup per README's Reproduction
  section (conda env `dlm`).
