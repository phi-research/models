# MNIST Superpixels — Graph Neural Network Demo

A small, self-contained demo that classifies MNIST digits **as graphs** rather than as pixel arrays:

1. Each MNIST image is segmented into superpixels with **SLIC**.
2. Each superpixel becomes a node; spatially adjacent superpixels are connected by edges.
3. A **Graph Convolutional Network (GCN)** — and a **Spline Convolutional Network** as an alternative — is trained to classify the resulting graphs.

This demo accompanies the lab's image-based GNN survey work and is referenced by:

> **Survey of Image Based Graph Neural Networks.**
> Usman Nazir, He Wang, Murtaza Taj. arXiv preprint, 2021.
> [arXiv:2106.06307](https://arxiv.org/abs/2106.06307)

It is intentionally minimal — read it as a hands-on companion to the survey, not as a research baseline.

---

## Repository Layout

```
MNIST_Superpixel_GCN/
├── MNIST_Superpixel_GCN.ipynb   # SLIC superpixels → GCN classifier
├── Spline_Convolution.ipynb     # Same task with B-spline graph convolutions
├── requirements.txt
└── README.md
```

---

## What the Notebooks Show

- **`MNIST_Superpixel_GCN.ipynb`** — Builds graphs with `torch_geometric.transforms.ToSLIC`, runs a `GCNConv` stack with global mean/max pooling, and reports test accuracy.
- **`Spline_Convolution.ipynb`** — Same dataset, swapping `GCNConv` for `SplineConv`, which conditions filter weights on edge attributes (relative position between superpixels). Useful for comparing message-passing variants on the same graphs.

---

## Reproducing the Demo

### 1. Environment

PyTorch Geometric needs PyTorch's wheel index for its companion packages (`torch-scatter`, `torch-sparse`, `torch-cluster`, `torch-spline-conv`). The simplest install:

```bash
python -m venv .venv
source .venv/bin/activate          # macOS / Linux
.venv\Scripts\activate             # Windows

pip install -r requirements.txt
# Then PyG companions matched to your torch / CUDA build:
TORCH=$(python -c "import torch; print(torch.__version__)")
CUDA=$(python -c "import torch; print('cu'+torch.version.cuda.replace('.','') if torch.cuda.is_available() else 'cpu')")
pip install --no-index --find-links "https://data.pyg.org/whl/torch-${TORCH}+${CUDA}.html" \
    torch-scatter torch-sparse torch-cluster torch-spline-conv
```

(See the [PyG installation guide](https://pytorch-geometric.readthedocs.io/en/latest/install/installation.html) if your platform needs different wheels.)

### 2. Run

```bash
jupyter notebook MNIST_Superpixel_GCN.ipynb
jupyter notebook Spline_Convolution.ipynb
```

Both notebooks download MNIST automatically the first time.

---

## References

- Survey: [arXiv:2106.06307](https://arxiv.org/abs/2106.06307)
- PyTorch Geometric `ToSLIC` transform: [docs](https://pytorch-geometric.readthedocs.io/en/latest/modules/transforms.html#torch_geometric.transforms.ToSLIC)
- Background reading: [arXiv:1907.09000](https://arxiv.org/abs/1907.09000), [arXiv:2002.05544](https://arxiv.org/abs/2002.05544)
- Walkthrough: [Understanding Graph Neural Networks (Medium)](https://medium.com/@rtsrumi07/understanding-graph-neural-network-with-hands-on-example-part-2-139a691ebeac)

---

## Contact

- **Lab:** [Planetary Health Informatics Lab](https://www.ndorms.ox.ac.uk/research/research-groups/planetary-health-informatics-1), NDORMS, University of Oxford
- **Head of Lab:** [Dr Sara Khalid](https://www.ndorms.ox.ac.uk/team/sara-khalid)
- **GitHub:** [@phi-research](https://github.com/phi-research)

---

## License

MIT — see the top-level [LICENSE](../LICENSE).
