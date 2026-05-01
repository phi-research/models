# Papers — Oxford Planetary Health Informatics Lab

Code, data references, and reproducibility materials accompanying peer-reviewed and pre-print publications from the **Oxford Planetary Health Informatics Lab (PHI Lab)**.

This repository is the canonical home for the analytical pipelines, statistical models, machine-learning code, and figure-generation scripts behind our publications. Each paper lives in its own subfolder with a self-contained `README.md` documenting data sources, dependencies, and how to reproduce the results.

---

## About the Lab

The [Planetary Health Informatics Lab](https://www.ndorms.ox.ac.uk/research/research-groups/planetary-health-informatics-1), based in the Nuffield Department of Orthopaedics, Rheumatology and Musculoskeletal Sciences (NDORMS) at the University of Oxford, is led by [Dr Sara Khalid](https://www.ndorms.ox.ac.uk/team/sara-khalid).

The lab develops open, reproducible, and policy-relevant computational tools to investigate the linkages between the environment, climate, and human health. We combine Earth observation, climate data, epidemiology, and machine learning to study how environmental change affects human health and to translate evidence into action.

---

## Repository Structure

```
papers/
├── <paper-short-name>/        # One folder per paper
│   ├── README.md              # Paper-specific overview, citation, reproduction steps
│   ├── data/                  # Data references (raw data is not tracked)
│   ├── src/ or notebooks/     # Analysis code
│   └── figures/               # Output figures
└── README.md                  # You are here
```

Each paper folder follows the same template so results can be reproduced consistently.

---

## Index of Papers

| Year | Title | Folder | Status |
|------|-------|--------|--------|
| 2026 | M-LSTM — Multivariate LSTM for Malaria Incidence Forecasting using Earth-Observation Indicators in South Asia | [`M-LSTM/`](M-LSTM) | Code released |

> More papers will be added here as they are released.

---

## Reproducing a Paper

```bash
git clone https://github.com/phi-research/papers.git
cd papers/<paper-short-name>
# Follow the local README.md for environment setup and run instructions
```

Most papers use Python (≥3.10) or R; environment files (`requirements.txt`, `environment.yml`, or `renv.lock`) are provided per project.

---

## Citation

If you use code from this repository, please cite the corresponding paper. Citation details (BibTeX and plain text) are provided inside each paper's local `README.md`.

---

## Contact

- **Lab:** [Planetary Health Informatics Lab](https://www.ndorms.ox.ac.uk/research/research-groups/planetary-health-informatics-1), NDORMS, University of Oxford
- **Head of Lab:** [Dr Sara Khalid](https://www.ndorms.ox.ac.uk/team/sara-khalid)
- **GitHub:** [@phi-research](https://github.com/phi-research)
- **Email:** orms1036@ox.ac.uk

For data access requests, collaborations, or questions about specific papers, please open an issue or contact the lab directly.

---

## License

Code in this repository is released under the **MIT License** unless a paper folder specifies otherwise. Data and figures may be subject to separate licensing — see individual paper folders for details.

---

_Open science for a healthier planet._
