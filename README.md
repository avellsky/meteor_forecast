# meteor_forecast

Meteor shower forecast data sets — the computed output of dust-trail
simulations, published so the results behind the papers can be inspected and
reused.

**Data only.** The model source code is not part of this repository, and
neither is the application that displays the files.

## Releases

| Directory | Contents |
|---|---|
| [`leonids-2030s/`](leonids-2030s/) | Leonid dust trails: the 2030s forecasts and the 1998–2009 hindcasts behind Abe (2026) — the epoch-matched simulations, the encounters evaluated from them, the 55P element sets their ejection sites were anchored to, and the ensembles behind the paper's calibration, orbit-covariance and systematics sections |

Each release directory carries its own `README.md` describing the files, a
`MANIFEST.json` with SHA-256 checksums and provenance, and a `LICENSE.md`.

## Opening the files

The datasets are in the format read by **Meteorium**, the meteor dust-trail
app on the App Store. Download a `.json` file and open it with the app —
Finder's *Open With*, or *Share → Meteorium* on iOS — and the simulation is
displayed as it was computed. The app makes no network connections; the file
you opened is the only thing it reads.

The files are also plain JSON with documented fields, so they can be read
directly without the app. See the release's `README.md` for the schema.

Contact: avellsky@gmail.com
