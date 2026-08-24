# Leonid dust trails in the 2030s — data release

Result files behind the Leonid forecasts and hindcasts of

> Abe, S. *Leonid dust trails in the 2030s* (submitted to Icarus)

This repository holds **data only**: the output of the integrations — the grain
states, the parent element sets they were anchored to, the encounter forecasts
evaluated from them, and the ensembles behind the paper's uncertainty and
systematics sections. Neither the model source code nor the viewer application
is part of this release.

The files are in the dataset format read by **Meteorium**, the free meteor
dust-trail app on the App Store, so the simulations can be inspected in the app
rather than only read as numbers.

## What is in `data/`

### Stream simulations

Each file is self-describing: its `params` header records the grain count,
ejection model, epoch, force model and the ephemeris-anchoring flag it was
generated with.

| File | Epoch | Trails | Grains | Used for |
|---|---|---|---|---|
| `LEO-epoch1998.json` | 1998 | 4 (1899–1998) | 100,000 | the 1998 encounter |
| `LEO-epoch1999.json` | 1999 | 4 (1899–1998) | 100,000 | the 1999 storm |
| `LEO-epoch2000.json` | 2000 | 10 (1699–1998) | 100,000 | the 2000 encounter |
| `LEO-epoch2001.json` | 2001 | 10 (1699–1998) | 100,000 | the 2001 storm |
| `LEO-epoch2002.json` | 2002 | 10 (1699–1998) | 100,000 | the 2002 twin peaks |
| `LEO-theory-epoch2009.json` | 2009 | 17 (1466–1998) | 170,000 | the 2009 encounter |
| `LEO-theory.json` | 2026 | 18 (1433–1998) | 180,000 | the interactive single-epoch model |

Why one dataset per historical year: the grains are archived as frozen two-body
elements, which carry no planetary perturbations. Propagating them far from
their epoch shifts the node distances by enough to misattribute the dominant
trail, so each encounter is evaluated with the simulation whose epoch matches
it. Measured, that omitted motion moves the node of an old trail by
10⁻² au over four years — tens of times the width of the section Earth
crosses — which is why the 2030s forecasts are **not** read from
`LEO-theory.json` either.

### Epoch-matched forecasts for 2030–2035 (`forecast/`)

One N-body integration per return, snapshotted at each forecast year and
merged into one dataset per year, so that every year is evaluated at its own
epoch. These are the datasets behind the 2031–2035 tables and figures of the
paper.

| File | Epoch | Trails | Grains |
|---|---|---|---|
| `LEO-fc2030.json` | 2030-11-18 | 18 (1433–1998) | 450,000 |
| `LEO-fc2031.json` | 2031-11-18 | 18 | 450,000 |
| `LEO-fc2032.json` | 2032-11-17 | 18 | 450,000 |
| `LEO-fc2033.json` | 2033-11-18 | 18 | 450,000 |
| `LEO-fc2034.json` | 2034-11-19 | 18 | 450,000 |
| `LEO-fc2035.json` | 2035-11-19 | 18 | 450,000 |

`LEO-theory.json` is kept because it is the dataset the app carries. It
reproduces the hindcast sequence, not the 2030s table.

### Encounter forecasts

`<dataset>-forecast.json` holds the evaluated encounters — the ZHR profile, the
per-trail contributions, the nodal geometry (`nodes`, `nodal_scatter`), the
Earth's path and the confidence class — for the years the paper reports.

| File | Years |
|---|---|
| `LEO-epoch1998-forecast.json` … `LEO-epoch2002-forecast.json` | 1998 … 2002, one each |
| `LEO-theory-forecast.json` | 2030–2035, evaluated from the single 2026-epoch dataset |

Each bundle is keyed by year under `years`, and records in `generated_from`
which simulation it came from. `LEO-theory-forecast.json` is the single-epoch
evaluation the app produces; the paper's 2030s numbers come from the per-year
datasets in `forecast/` instead, for the reason given above.

### The 1999 anchor: realisations and grain count (`calibration/`)

The calibration constant is fixed on the 1999 storm, so how far that anchor
moves when nothing but the random ejection sample changes, and how far it is
still moving with grain count, both bound it.

| File | What it holds |
|---|---|
| `leo1999_realizations.json` | the four realisations, summarised: ZHR, time, node, and the factor 1.50 they span |
| `LEO-real-4{2,3,4,5}-8000.json` | the four simulations themselves, seeds 42–45, 8,000 grains per return |
| `leo1999_convergence.json` | ZHR against grain count, 1,000 → 25,000, three draws each |
| `LEO-theory-e1999hi.json` | the 250-year, 8-return, 30,000-grain run the convergence series is measured against |

### Orbit-covariance Monte Carlo (`covariance/`)

Twelve members drawn from the full 8-dimensional 55P orbit solution covariance
of the paper's Table 2 — including A₁ and A₂, plus a lognormal
inter-apparition non-gravitational model error — each carried through the
complete trail simulation at 32,000 grains, and four controls that repeat the
nominal orbit with independent ejection samples.

| File | What it holds |
|---|---|
| `leo1999_covariance_mc_hi.json` | the drawn orbit, the measured node and the rate for every member, plus the ensemble summary |
| `LEO-mc-cov01.json` … `LEO-mc-cov12.json` | the twelve members' trail snapshots |
| `LEO-mc-nominal.json`, `LEO-mc-seed01.json` … `seed04.json` | the ejection controls |

Each member is a complete simulation, so its node can be re-measured without
repeating the integration.

### Residual systematics (`systematics/`)

The experiments behind the paper's Sect. 3.4 — what a stored snapshot omits,
which forces omit it, and what the oldest trails do about it.

| File | What it holds |
|---|---|
| `leo_frozen_element_test.json` | frozen-element against N-body propagation to the 2034 encounter, trail by trail |
| `leo_frozen_element_forces.json` | the node displacement between the 2030 and 2034 snapshots, and the split between planetary and radiative forces |
| `leo_old_trails.json` | the oldest trails evaluated from datasets snapshotted at each encounter year |
| `leo_epoch_control.json` | the same trails from the 2026-11-17 snapshot — the control |
| `leo_anchor_realizations.json` | the ejection-realisation spread of the 1767 trail in the 2001 and 2002 hindcasts |
| `LEO-t1633-*.json`, `LEO-t1666-*.json`, `LEO-t1699-*.json` | the single-trail simulations those measurements are made on, 25,000 grains each |
| `LEO-t1433-*.json` | the 1433 trail at 2030, at 2034, and from the 2026 snapshot |
| `LEO-t1767-{2001,2002}-s4{2,3,4,5}-10000.json` | the 1767 trail, four ejection seeds, at each storm year |

The `-A-`/`-B-` pair and the `-frozen-from-2026`/`-nbody-to-2034` pair are the
two arms of the same comparison: one integration written out at two epochs,
against the same grains propagated with frozen elements.

### Population index (`population/`)

`leo_population_index.json` holds the population-index and limiting-magnitude
conversion of the paper's Sect. 3.3, per shower and per source.

### Parent element sets

`parents.json` holds the 55P/Tempel-Tuttle element sets the integrations start
from, retrieved from the JPL Small-Body Database at full precision, including
the full 8×8 covariance matrix (`sbdb.api?sstr=55P&cov=mat`) used for the
ensemble in the paper.

### Index

`index.json` is the dataset index in Meteorium's own format, listing the six
simulations with their identifiers, grain counts and shower code.

## File format

Each simulation file is a single JSON object:

```
id, name, name_ja, kind, shower   identification
epoch_jd                          epoch of the archived elements
parent                            55P elements at the same epoch
params                            everything the run was generated with
trails[]                          one entry per ejection apparition
  label                             the apparition year, e.g. "1932"
  particles                         parallel arrays, one entry per grain
```

Every array under `particles` has the same length — one value per grain:

| Field | Meaning |
|---|---|
| `a` | semi-major axis (AU) |
| `e` | eccentricity |
| `i` | inclination (deg) |
| `node` | longitude of the ascending node (deg) |
| `peri` | argument of perihelion (deg) |
| `M` | mean anomaly at `epoch_jd` (deg) |
| `beta` | radiation-pressure parameter β |
| `radius` | grain radius (m) |
| `weight` | relative weight in the ZHR sum |

Elements are heliocentric ecliptic J2000; units are AU / day / GM with G = 1.
Grains are propagated as Kepler motion from `epoch_jd`, which is what makes the
epoch-matched datasets necessary.

## Model summary

- Integrator: REBOUND (IAS15) with REBOUNDx radiation forces
- Ejection: Crifo & Rodionov (1997) gas-drag terminal velocity, grain sizes
  drawn directly from dN ∝ a<sup>−u</sup>, β = 5.74×10⁻⁴/(ρa), β ≤ 3.0×10⁻³
- Ejection sites anchored to the JPL Horizons ephemeris of 55P for each
  apparition, rather than to an internal backward integration (paper Sect. 2.2)
- Empirical density factor for old trails: 0.11

`MANIFEST.json` lists every file with its SHA-256 checksum and the provenance
fields read back out of its header.

## Layout and checksums

`MANIFEST.json` lists every file with its SHA-256 checksum and the provenance
fields read back out of its header. Paths in it are relative to it; on Zenodo
the same files are deposited flat, under the basename recorded as
`zenodo_name`, and the basenames are unique across the release.

A few dataset headers carry a `params.built_by` string naming the script that
wrote them. Those scripts are not part of this release — the string is
provenance, not a pointer to something downloadable. The model is specified in
the paper, including the numerical value of every constant it uses.

## Reuse

Released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) —
please cite the paper. See `LICENSE.md`.

Contact: avellsky@gmail.com
