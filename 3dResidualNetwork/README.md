# 3D Residual Network — Spatio-temporal Analysis of Remote Sensing Data

This folder contains the code accompanying:

> **Using 3D Residual Network for Spatio-temporal Analysis of Remote Sensing Data.**
> Usman Nazir, Mohsen Ali, Murtaza Taj, Numan Khurshid.
> *IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP)*, 2019.
> [DOI: 10.1109/ICASSP.2019.8682286](https://ieeexplore.ieee.org/document/8682286)

The paper compares 2D and 3D residual networks for classifying multi-temporal Sentinel-2 image stacks (a time-series of satellite tiles for the same patch of land), showing that 3D convolutions across the time axis improve land-cover / change classification compared to single-image 2D baselines.

---

## Repository Layout

```
3dResidualNetwork/
├── 3dresnet.ipynb              # 3D ResNet — spatio-temporal model (paper)
├── 3dresnet annotate.ipynb     # 3D ResNet inference / annotation pipeline
├── 2dresnet_annotate.ipynb     # 2D baseline annotation pipeline
├── 2dNasNet.ipynb              # NASNet 2D baseline
├── 2dannotator.ipynb           # 2D image annotator UI
├── requirements.txt
└── README.md
```

The two annotators are tools for labelling new tiles; the model notebooks consume the resulting label sets.

---

## Model

- **Backbone.** Standard ResNet residual blocks, lifted from 2D `Conv2D` to 3D `Conv3D` so the kernel runs over `(time, height, width)`.
- **Input.** A short stack of co-registered satellite tiles for the same geographic patch (e.g. 5–10 timestamps × `H × W × C`).
- **Head.** Global average pooling → fully connected → softmax over land-cover / change classes.

The 2D NASNet and 2D ResNet notebooks are included as the single-timestamp baselines reported in the paper.

---

## Reproducing the Results

### 1. Environment

```bash
python -m venv .venv
source .venv/bin/activate          # macOS / Linux
.venv\Scripts\activate             # Windows
pip install -r requirements.txt
```

### 2. Prepare the data

The original paper used Sentinel-2 multi-temporal stacks. Place per-class folders of `(T, H, W, C)` `.npy` arrays — or a folder of co-registered `.tif` tiles per timestamp — under the path the notebook expects (edit the data-loader cell at the top).

### 3. Train

```bash
jupyter notebook 3dresnet.ipynb         # 3D model — main result
jupyter notebook 2dNasNet.ipynb         # 2D baseline
```

### 4. Annotate / inspect predictions

```bash
jupyter notebook 3dresnet\ annotate.ipynb
jupyter notebook 2dresnet_annotate.ipynb
```

---

## Citation

```bibtex
@inproceedings{nazir20193dresnet,
  author    = {Nazir, Usman and Ali, Mohsen and Taj, Murtaza and Khurshid, Numan},
  title     = {Using 3D Residual Network for Spatio-temporal Analysis of Remote Sensing Data},
  booktitle = {2019 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP)},
  pages     = {1403--1407},
  year      = {2019},
  doi       = {10.1109/ICASSP.2019.8682286}
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
