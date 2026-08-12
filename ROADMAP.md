# Roadmap - planned and parked work

The published core (README) answers one question: gradual drift vs. discrete breaks
in safe-haven betas, via a fixed-variance dynamic linear model. Everything below
either extends that core or was deliberately left open. Nothing here is required to
read or reproduce the core result.

Each item notes **how far it got** and **where the code lives**, so the project can
be picked up later without re-reading everything.

## 1. Stochastic volatility in the observation equation
**Status: implemented and converged.** Notebooks `06_sv.ipynb` (single-currency
motivation: residual kurtosis drops 9.5 -> 3.6 after SV) and `07_tvp_sv.ipynb`
(joint TVP-SV, all four currencies, NUTS/PyMC). Outputs in
`data/tvpsv_<CUR>_*.csv` and `figures/tvpsv_*`. Kept as an extension rather than in
the core to keep the headline narrative simple.
**Next:** a final run with `DRAWS = 2000` (current min ESS ~120-600, weakest for
GBP; adequate for means/bands, tight for a submission-grade result).

## 2. Maximum-likelihood estimation of the state variance
**Status: deliberately avoided in the core; fixed lambda used instead.** The MLE of
the state-to-observation variance ratio suffers from the pile-up problem, the
likelihood peaks at the boundary (zero time variation), so the estimate collapses
to "no drift". Candidate fixes to explore: a Bayesian prior on the ratio (partial
pooling away from the boundary), a restricted parameter space, or a
reparameterisation. The TVP-SV extension already estimates the volatility
parameters within a Bayesian frame, which is the natural place to revisit this.

## 3. Genuine jumps rather than pure drift
**Status: conceptual, not implemented.** The random-walk state equation can only
produce gradual drift, yet events like the 2015 SNB de-peg are discontinuous. This
is the core tension the project raises but does not resolve. Candidate models:
a state equation with an added jump component, Markov-switching / regime-switching
coefficients, or a mixture-innovation state space. This is the most promising
direction for a thesis-length continuation.

## 4. Bayesian in-depth treatment
**Status: entry point exists via the TVP-SV code.** A fully Bayesian DLM (priors on
the state variances, posterior over the whole coefficient path) is the natural
anchor for an in-depth study on time-series methods. The PyMC machinery in
notebook 07 is the starting point.

## 5. Out-of-sample / predictive evaluation
**Status: out of scope by design.** The whole project is in-sample structural
analysis. A predictive extension (recursive filtering, one-step forecasts, formal
break vs. drift model comparison) would be a separate study.
