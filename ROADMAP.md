# Roadmap - planned and parked work

The published core (README) answers one question: gradual drift vs. discrete breaks
in safe-haven betas, via a fixed-variance dynamic linear model. Everything below
either extends that core or was deliberately left open. Nothing here is required to
read or reproduce the core result.

Each item notes the idea and where it would fit, so the project can be picked up
later without re-reading everything.

## 1. Stochastic volatility in the observation equation
**Status: planned; the most direct extension.** The core DLM assumes a constant
observation variance, yet the innovation diagnostics (notebook 05) show volatility
clustering and fat tails that a constant variance cannot capture. Letting the
observation variance vary over time (stochastic volatility), estimated in a Bayesian
frame such as a joint TVP-SV model via MCMC, is the most direct way to relax that
assumption, and would be the first thing a continuation builds.

## 2. Maximum-likelihood estimation of the state variance
**Status: deliberately avoided in the core; fixed lambda used instead.** The MLE of
the state-to-observation variance ratio suffers from the pile-up problem, the
likelihood peaks at the boundary (zero time variation), so the estimate collapses
to "no drift". Candidate fixes to explore: a Bayesian prior on the ratio (partial
pooling away from the boundary), a restricted parameter space, or a
reparameterisation. A Bayesian treatment of the variance ratio (as in the
stochastic-volatility direction above) is the natural place to revisit this.

## 3. Genuine jumps rather than pure drift
**Status: conceptual, not implemented.** The random-walk state equation can only
produce gradual drift, yet events like the 2015 SNB de-peg are discontinuous. This
is the core tension the project raises but does not resolve. Candidate models:
a state equation with an added jump component, Markov-switching / regime-switching
coefficients, or a mixture-innovation state space. This is the most promising
direction for a thesis-length continuation.

## 4. Bayesian in-depth treatment
**Status: planned.** A fully Bayesian DLM (priors on the state variances, a posterior
over the whole coefficient path) would be the natural anchor for an in-depth study of
time-series methods, building on the stochastic-volatility direction above.

## 5. Out-of-sample / predictive evaluation
**Status: out of scope by design.** The whole project is in-sample structural
analysis. A predictive extension (recursive filtering, one-step forecasts, formal
break vs. drift model comparison) would be a separate study.
