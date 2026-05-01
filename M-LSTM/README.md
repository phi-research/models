# M-LSTM — Multivariate LSTM for Malaria Incidence Forecasting

This folder contains the code, data, and reproducibility materials for the **M-LSTM (Multivariate Long Short-Term Memory)** model used to forecast malaria incidence in South Asia (India, Bangladesh, Pakistan) by combining DHS health survey data with Earth-observation indicators (NDVI, nightlights, land-surface temperature, rainfall).

---

## Overview

Malaria incidence is influenced by a combination of environmental drivers — vegetation, temperature, rainfall, and human activity proxies (nightlights). This study trains a multivariate LSTM that ingests:

- **Health outcome:** DHS-derived malaria indices per cluster
- **Earth-observation features:**
  - **NDVI** — vegetation index
  - **NTL** — VIIRS / DMSP nightlights (human activity)
  - **LST** — land-surface temperature
  - **Rainfall** — precipitation

…and forecasts malaria incidence at the cluster-year level. The pipeline is split into two notebooks:

| Notebook | Purpose |
|----------|---------|
| `DataPrep.ipynb` | Loads per-country DHS + EO tables, aligns them by cluster/year, reshapes wide-format yearly columns into a long time-series (`series_to_supervised`), and writes `data/reshaped_data/<Country>_reframed.csv`. |
| `M_LSTM.ipynb` | Loads the reframed data, normalises features, splits train/test, defines the `MultivariateLSTM` PyTorch model, trains it, evaluates with MAE / MSE / RMSE, and logs results to `Model_log.csv`. |

---

## Repository Layout

```
M-LSTM/
├── DataPrep.ipynb              # Data preparation pipeline
├── M_LSTM.ipynb                # Model training and evaluation
├── data/
│   └── reshaped_data/          # Output of DataPrep — input to M_LSTM
│       ├── Bangladesh_reframed.csv
│       ├── India_reframed.csv
│       └── Pakistan_reframed.csv
├── requirements.txt            # Python dependencies
└── README.md                   # You are here
```

> ⚠️ **Note on data:** The reframed CSVs (especially `India_reframed.csv` ≈ 175 MB) are larger than GitHub's 100 MB single-file limit and are excluded by `.gitignore`. They are reproducible from `DataPrep.ipynb` given the raw DHS + EO tables (see **Data sources** below). If you need the pre-built CSVs, contact the lab or use Git LFS / Zenodo for distribution.

---

## Data Sources

The raw inputs to `DataPrep.ipynb` are not redistributed in this repository — they are obtained from the original providers:

| Variable | Source | Access |
|----------|--------|--------|
| Malaria indices, cluster GPS, DHS region | [DHS Program](https://dhsprogram.com/) | Free with registration |
| NDVI | MODIS (MOD13Q1) via Google Earth Engine | Open |
| Nightlights (NTL) | DMSP-OLS / VIIRS via Google Earth Engine | Open |
| Land-surface temperature | MODIS (MOD11A2) via Google Earth Engine | Open |
| Rainfall | CHIRPS via Google Earth Engine | Open |

Place the per-country pre-extracted CSVs under `Data Saperation/<Country>/` matching the filenames referenced in `DataPrep.ipynb` (e.g. `India_ndvi.csv`, `India_ntl.csv`, `India_temperature.csv`, `India_rainfall.csv`, plus the malaria table). The DataPrep notebook then aligns and reshapes them into `data/reshaped_data/<Country>_reframed.csv`.

---

## Reproducing the Results

### 1. Set up the environment

```bash
# From this folder
python -m venv .venv
source .venv/bin/activate          # macOS / Linux
.venv\Scripts\activate             # Windows
pip install -r requirements.txt
```

Tested on Python **3.10+**. PyTorch is CPU-friendly for the model sizes used here; a GPU is supported but not required.

### 2. Prepare the data

Place the raw per-country CSVs under `Data Saperation/<Country>/` as listed above. Then open and run:

```bash
jupyter notebook DataPrep.ipynb
```

Set `country = 'India'` (or `'Bangladesh'` / `'Pakistan'`) in cell 4 and run all cells. The output `data/reshaped_data/<Country>_reframed.csv` is the input to the next notebook.

### 3. Train and evaluate the model

```bash
jupyter notebook M_LSTM.ipynb
```

Set `country` in cell 2 to match the country prepared above. Hyperparameters (number of layers, hidden size, epochs, learning rate) are defined near the top of the model cells — adjust as needed. Training writes:

- Checkpoints to `params/`
- Loss plots to `Graphs/loss/`
- A row to `Model_log.csv` with the final MAE / MSE / RMSE

### 4. Compare runs

`Model_log.csv` accumulates results across runs (country, epochs, layers, hidden size, losses) for easy comparison.

---

## Model

The model is a stacked `MultivariateLSTM` (PyTorch `nn.LSTM` + linear head) that consumes a fixed-length window of multivariate features per cluster and predicts the next-step malaria index.

Key design choices:
- **Inputs** are normalised to `[0, 1]` before training.
- `series_to_supervised(...)` (in `DataPrep.ipynb`) shifts wide yearly columns into supervised `(X_t, y_{t+1})` pairs.
- Default loss: MSE; reported metrics: MAE, MSE, RMSE.
- Reproducibility: seeds for `random` and `torch` are set at the top of `M_LSTM.ipynb`.

---

## Requirements

See [`requirements.txt`](requirements.txt). Core dependencies:

- `pandas`, `numpy` — tabular data
- `scikit-learn` — train/test split, metrics, label encoding
- `matplotlib` — loss / diagnostic plots
- `torch` — model, training loop
- `jupyter` — to run the notebooks

---

## Citation

If you use this code or build on this work, please cite the corresponding paper. A BibTeX entry will be added here once the paper is published.

```bibtex
@article{phi_lab_mlstm,
  title   = {Multivariate LSTM for Malaria Incidence Forecasting using Earth-Observation Indicators in South Asia},
  author  = {Planetary Health Informatics Lab, University of Oxford},
  year    = {2026},
  note    = {Code: https://github.com/phi-research/models/tree/main/M-LSTM}
}
```

---

## Contact

- **Lab:** [Planetary Health Informatics Lab](https://www.ndorms.ox.ac.uk/research/research-groups/planetary-health-informatics-1), NDORMS, University of Oxford
- **Head of Lab:** [Dr Sara Khalid](https://www.ndorms.ox.ac.uk/team/sara-khalid)
- **Email:** orms1036@ox.ac.uk

For data access requests or reproducibility questions, please open an issue on the [models repository](https://github.com/phi-research/models).

---

## License

MIT — see the top-level [LICENSE](../LICENSE) file.
