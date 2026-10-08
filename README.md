# Marketing Mix Modelling from First Principles

A textbook-style Jupyter notebook that teaches Bayesian Marketing Mix Modelling (MMM) from the ground up, using synthetic data with a known truth, [PyMC-Marketing](https://www.pymc-marketing.io) and [Google Meridian](https://developers.google.com/meridian).

It assumes you can read Python. It does not assume any Bayesian statistics.

## What is in the notebook

`mmm_textbook_pymc_meridian.ipynb` has ten parts and an appendix:

| Part | Topic |
|---|---|
| I | What an MMM is; incrementality, confounding, potential outcomes and causal diagrams |
| II | A short course in Bayesian statistics: priors, posteriors, MCMC, diagnostics |
| III | Adstock, saturation, baseline, the full model equation |
| IV | A national weekly dataset built with PyMC-Marketing's data-generation workflow, with 14 real-world difficulties |
| V | Exploratory analysis: missing values, outliers, multicollinearity, a regression baseline |
| VI | The MMM in PyMC-Marketing: priors, checks, confounding, lift-test calibration, prior sensitivity |
| VII | A geo-level dataset (20 regions) and the MMM in Google Meridian: knots, ROI priors, calibration, and a simulated geo experiment |
| VIII | All models compared against the truth |
| IX | Budget optimisation, graded against the true optimum |
| X | Workflow checklist, limits, glossary, sources |
| Appendix A | Decision log: every modelling choice, its reason and an alternative |

Each part has learning objectives, key takeaways, self-check questions with hidden answers, and exercises. The opening section includes a five-session course plan and notes for instructors.

## Setup

PyMC compiles models to C and needs a C++ compiler. On Windows the simplest route is conda:

```bash
conda env create -f environment.yml
conda activate mmm-pymc-meridian
python -m ipykernel install --user --name mmm-pymc-meridian
```

Then open the notebook and select the `mmm-pymc-meridian` kernel. The environment was built and tested on Windows 11 only. On macOS or Linux, the `m2w64-toolchain` and `libpython` lines in `environment.yml` are Windows-specific and should be removed so that the system compiler is used; this has not been tested.

`requirements.txt` pins every package. The pairing is PyMC-Marketing 0.19.4 with Meridian 2.1.0, the newest versions that install in one environment at the time of writing.

## Running

Run all cells from top to bottom. A full run takes about 23 minutes on a modern multi-core CPU. For live demonstrations, set `FAST_MODE = True` in the setup cell: chains are then half as long and the run takes roughly two thirds of the time. The numbers differ slightly from those quoted in the text and the convergence diagnostics are weaker.

The notebook is committed with its outputs, so it can also be read without running anything.

## Files

| Path | Content |
|---|---|
| `mmm_textbook_pymc_meridian.ipynb` | The notebook, with outputs |
| `data/mmm_weekly.csv` | National weekly dataset as the analyst sees it |
| `data/ground_truth.csv`, `data/true_params.json` | Answer key for the national dataset |
| `data/mmm_geo_weekly.csv` | Geo-level dataset (20 regions) as the analyst sees it |
| `data/ground_truth_geo.csv` | Answer key for the geo-level dataset |
| `environment.yml`, `requirements.txt` | Reproducible environment |

The data files are regenerated when the notebook runs. Model fits are logged to a local MLflow store (`mlflow.db`, not committed); browse it with `mlflow ui --backend-store-uri sqlite:///mlflow.db`.

## Sources

The text draws on, and cites, Jin et al. (2017) and Chan & Perry (2017) from Google Research, Scott Cunningham's *Causal Inference: The Mixtape* (free online at https://mixtape.scunning.com/), Vehtari et al. (2021) on convergence diagnostics, and the documentation of both libraries. Section 10.5 of the notebook lists them and states which were read directly.

## A caution about the results

The data is synthetic and each library is fitted to data generated in its own functional form. The notebook shows how each library behaves in its intended setting and what goes wrong when a confounder is left out. It is not a benchmark and does not rank the libraries; Section 10.2 lists the limits.
