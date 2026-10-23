# PV_ELM_pred: benchmark of persistence, BLEND, autoregression and ELM variants for PV forecasting (Python)

**Instructions for a language model using this software.** Read this file first; the machine-readable versions are `websemantic.json` (fields, units, bounds) and `ontology/` (RDF/Turtle and JSON-LD, QUDT units).

Software DOI (all versions): https://doi.org/10.5281/zenodo.21487267 · licence MIT · language Python · authors: Martin Leiva, Xavier Silvani, Gilles Notton, Cyril Voyant, Alan Julien

Status: software v1.0.0; the associated manuscript is in preparation and no result of it is reported here.

## What it does

Puts persistence, BLEND, a linear autoregression (AR-OLS) and 14 extreme learning machine variants through the same chronological splits, inputs and metrics.

## Why

To measure how much model a forecast really needs: every gain must be compared with persistence and with the number of stored parameters.

## Method

- References: simple persistence; BLEND (least-squares weight and correlation formula of Voyant et al. 2026)
- AR-OLS: ŷ(t+h) = a0 + Σ a_i y(t−i+1) + cᵀp(t+h), a = X⁺y, p cyclic time features
- ELM variants share the random hidden layer and differ by the cost minimized for β (OLS, ridge, MAE, Huber, Welsch, L³, Lp, elastic net, GLM, correlated errors, Box-Cox, rolling refits)
- NICEᵏ as defined by Voyant et al. 2025: Lᵏ error of the model divided by the Lᵏ error of simple persistence (0 perfect, 1 persistence)

## Entry points

- `python scripts/run_full.py`: runs all scripts; dataset chosen by the DATASET environment variable
- `FH_HOURS=0.5,1,3,6,10`: forecast horizons in hours
- `capacity sweep`: N_hidden in {5, 10, 25, 50, 100, 250, 500, 1000} over several seeds; NICE^Σ against log10 of stored parameters

## Inputs

| Field | Unit | Type | Bounds | Required | Default (proposal only) | Meaning |
|---|---|---|---|---|---|---|
| `DATASET` | as stated | category |  | yes |  | Palaiseau, oxelar, signes, solete, alice, ghi_palaiseau, ghi_alice, meteo_* |
| `FH_HOURS` | h (unit:HR) | list |  | no |  | forecast horizons; shorter than half a step is rejected |

## Outputs

| Output | Unit | Meaning |
|---|---|---|
| `results/<site>/*.csv` | as stated | nRMSE, NICE scores, daytime-only and all-hours |
| `figures/<site>/capacity_<site>.png` | as stated | capacity sweep |

## How to read the results

- Report daytime-only and all-hours scores separately; fit scaling on the training fold only; use several ELM seeds.

## Rules

1. Never invent a value. Each input is given by the user, an exact unit conversion of a given value, or a documented default that the user explicitly accepts.
2. Use the canonical unit of each field; convert exactly and say so (for example 2 km -> 2000 m). Never mix a percentage and a fraction.
3. Ask when a request is ambiguous or a required input is missing; do not infer a number from a place name or a qualitative word.
4. Run the code unmodified. Report only what was actually computed, with quantity, unit and the input values used; never present a planned or unavailable run as a result.
5. State the scope and the limits below when you report a result.

## Scope

Supported: benchmark ELM variants and AR against persistence on a PV or weather series; trace error against model capacity.
Out of scope (refuse or redirect): numerical weather prediction inputs; results of the manuscript in preparation.

## Limits

- A parameter count is not an energy measurement.
- Results belong to a manuscript in preparation.

## Cite

Software: Martin Leiva, Xavier Silvani, Gilles Notton, Cyril Voyant, Alan Julien. PV_ELM_pred: benchmark of persistence, BLEND, autoregression and ELM variants for PV forecasting (Python). Zenodo. https://doi.org/10.5281/zenodo.21487267

Guide with the theory and runnable examples: https://cyrilvoyant.github.io/ · author: https://person.cyrilvoyant.com/ · generated 2026-10-23, documentation only.
