# Changelog

## leonids-2030s-v1

First release. Holds the data behind every number the paper reports:

- **`LEO-epoch1998.json` … `LEO-epoch2002.json`, `LEO-theory-epoch2009.json`** —
  the epoch-matched simulations for the 1998–2002 and 2009 encounters.
- **`forecast/LEO-fc2030.json` … `LEO-fc2035.json`** — the epoch-matched
  datasets the 2031–2035 tables and figures are computed from, one per year,
  450,000 grains each.
- **`covariance/`** — the 8-dimensional orbit-covariance Monte Carlo, twelve
  members and four ejection controls at 32,000 grains each, member by member.
- **`calibration/`** — the ejection-realisation spread and the grain-count
  convergence of the 1999 anchor, with the simulations they are measured on.
- **`systematics/`** — the frozen-element, old-trail and anchoring experiments
  behind Sect. 3.4.
- **`population/`** — the population-index conversion of Sect. 3.3.
- **`LEO-theory.json`** and the `-forecast.json` bundles — the single-epoch
  model the app carries, and the evaluated encounters.
- **`parents.json`** — the 55P element sets, including the full 8×8 covariance
  matrix.
