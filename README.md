# Models — Oxford Planetary Health Informatics Lab

> _Model code accompanying PHI Lab publications._

Code, data references, and reproducibility materials accompanying peer-reviewed and pre-print publications from the **Oxford Planetary Health Informatics Lab (PHI Lab)**.

This repository is the canonical home for the analytical pipelines, statistical models, machine-learning code, and figure-generation scripts behind our publications. Each paper lives in its own subfolder with a self-contained `README.md` documenting data sources, dependencies, and how to reproduce the results.

---

## About the Lab

The [Planetary Health Informatics Lab](https://www.ndorms.ox.ac.uk/research/research-groups/planetary-health-informatics-1), based in the Nuffield Department of Orthopaedics, Rheumatology and Musculoskeletal Sciences (NDORMS) at the University of Oxford, is led by [Dr Sara Khalid](https://www.ndorms.ox.ac.uk/team/sara-khalid).

The lab develops open, reproducible, and policy-relevant computational tools to investigate the linkages between the environment, climate, and human health. We combine Earth observation, climate data, epidemiology, and machine learning to study how environmental change affects human health and to translate evidence into action.

---

## Repository Structure

```
models/
├── <model-short-name>/        # One folder per model / paper
│   ├── README.md              # Model overview, citation, reproduction steps
│   ├── data/                  # Data references (raw data is not tracked)
│   ├── src/ or notebooks/     # Analysis code
│   └── figures/               # Output figures
└── README.md                  # You are here
```

Each model folder follows the same template so results can be reproduced consistently.

---

## Index of Models

| Year | Title | Folder | Status |
|------|-------|--------|--------|
| 2026 | M-LSTM — Multivariate LSTM for Malaria Incidence Forecasting using Earth-Observation Indicators in South Asia | [`M-LSTM/`](M-LSTM) | Code released |
| 2026 | STAG-NN-BA — Spatio-Temporal Attention Graph Neural Networks for Brick Kilns in Asia (GCN / GAT / SplineConv on superpixel graphs) | [`STAG-NN-BA/`](STAG-NN-BA) | Code released |
| 2021 | Survey of Image-Based Graph Neural Networks (companion demo: MNIST superpixels → GCN / SplineConv) | [`MNIST_Superpixel_GCN/`](MNIST_Superpixel_GCN) | Demo released |
| 2020 | Kiln-Net — A Gated Neural Network for Detection of Brick Kilns in South Asia ([IEEE J-STARS](https://ieeexplore.ieee.org/document/9115879)) | [`KilnNet/`](KilnNet) | Code released |
| 2020 | MODALES — NOx emission cost comparison (simulator vs. mathematical model) for the EU H2020 MODALES project | [`MODALES/`](MODALES) | Code released |
| 2019 | Tiny-Inception-ResNet-v2 — Using Deep Learning for Eliminating Bonded Labors of Brick Kilns in South Asia ([arXiv:1907.05552](https://arxiv.org/abs/1907.05552)) | [`Tiny-Inception-ResNet-v2/`](Tiny-Inception-ResNet-v2) | Code released |
| 2019 | Using 3D Residual Network for Spatio-temporal Analysis of Remote Sensing Data ([ICASSP](https://ieeexplore.ieee.org/document/8682286)) | [`3dResidualNetwork/`](3dResidualNetwork) | Code released |

> More papers will be added here as they are released.

---

## Reproducing a Paper

```bash
git clone https://github.com/phi-research/models.git
cd models/<model-short-name>
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
