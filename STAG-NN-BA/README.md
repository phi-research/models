# STAG-NN-BA — Spatio-Temporal Attention Graph NN for Brick-kilns in Asia

This folder contains the code for the **STAG-NN-BA** family of graph neural networks evaluated on:

- **ASIA14** — a multi-country (14-region) brick-kiln satellite-tile dataset for South Asia, used for *kiln vs. no-kiln* graph classification on superpixel graphs.
- **C2D2** — a temporal-volume "change-detection" dataset where each example is a stack of co-registered tiles over time.

The implementation compares three graph-convolutional backbones — **GCN**, **GAT (v1)**, and **SplineConv** — on the same superpixel graph representation, with a separate GAT variant (`GATV1C2D2`) for the temporal C2D2 task.

---

## Repository Layout

```
STAG-NN-BA/
├── datasets/
│   ├── asia14.py                     # ASIA14 InMemoryDataset (auto-download + SLIC graphs)
│   └── c2d2.py                       # C2D2 temporal dataset
├── models/
│   ├── gcn.py                        # GCN classifier + train loop
│   ├── gatv1.py                      # GATv1 classifier
│   ├── gatv1_c2d2.py                 # GATv1 variant for the C2D2 temporal volumes
│   └── spline.py                     # SplineConv classifier + train loop
├── notebooks/
│   ├── prepare_superpixels_ASIA14.ipynb     # Build SLIC graphs for ASIA14
│   ├── c2d2_temporal_volume_processing.ipynb # Build temporal volumes for C2D2
│   └── main.ipynb                            # End-to-end train / eval
├── requirements.txt
└── README.md
```

---

## Models

| Model         | File                  | Notes |
|---------------|-----------------------|-------|
| GCN           | `models/gcn.py`       | 4× `GCNConv` + global mean/max pool concat + linear head. |
| GATv1         | `models/gatv1.py`     | Configurable layer sizes / attention heads + 2-layer MLP head. |
| GATv1 (C2D2)  | `models/gatv1_c2d2.py`| Same architecture, applied to temporal-volume graphs. |
| SplineConv    | `models/spline.py`    | Two `SplineConv` blocks with `graclus` pooling; LR step-decay at epoch 16 / 26. |

Each model can be instantiated standalone (`python -m models.gcn` / etc.) or imported into `notebooks/main.ipynb`.

---

## Reproducing the Results

### 1. Environment

PyTorch Geometric requires its companion wheels (`torch-scatter`, `torch-sparse`, `torch-cluster`, `torch-spline-conv`) — install them from the PyG index that matches your torch + CUDA build:

```bash
python -m venv .venv
source .venv/bin/activate          # macOS / Linux
.venv\Scripts\activate             # Windows

pip install -r requirements.txt
TORCH=$(python -c "import torch; print(torch.__version__)")
CUDA=$(python -c "import torch; print('cu'+torch.version.cuda.replace('.','') if torch.cuda.is_available() else 'cpu')")
pip install --no-index --find-links "https://data.pyg.org/whl/torch-${TORCH}+${CUDA}.html" \
    torch-scatter torch-sparse torch-cluster torch-spline-conv
```

### 2. Build the graphs

```bash
jupyter notebook notebooks/prepare_superpixels_ASIA14.ipynb
jupyter notebook notebooks/c2d2_temporal_volume_processing.ipynb
```

`asia14.py` downloads the raw ASIA14 archive on first use; `c2d2.py` expects pre-built `train.pkl` / `test.pkl` produced by the temporal-volume notebook.

### 3. Train and evaluate

```bash
jupyter notebook notebooks/main.ipynb
```

Pick the model, dataset, and hyperparameters near the top of the notebook.

---

## Citation

This work is part of the lab's brick-kiln detection programme (Tiny-Inception-ResNet-v2, Kiln-Net, and the GNN survey referenced in `MNIST_Superpixel_GCN`). A standalone STAG-NN-BA reference will be added once the manuscript is publicly available.

```bibtex
@misc{phi_lab_stag_nn_ba,
  title  = {STAG-NN-BA: Spatio-Temporal Attention Graph Neural Networks for Brick Kilns in Asia},
  author = {Planetary Health Informatics Lab, University of Oxford},
  year   = {2026},
  note   = {Code: https://github.com/phi-research/models/tree/main/STAG-NN-BA}
}
```

---

## Contact

- **Lab:** [Planetary Health Informatics Lab](https://www.ndorms.ox.ac.uk/research/research-groups/planetary-health-informatics-1), NDORMS, University of Oxford
- **Head of Lab:** [Dr Sara Khalid](https://www.ndorms.ox.ac.uk/team/sara-khalid)
- **GitHub:** [@phi-research](https://github.com/phi-research)

---

## License

MIT — see the top-level [LICENSE](../LICENSE).
