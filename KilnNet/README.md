# Kiln-Net — Detection of Brick Kilns in South Asia

This folder contains the code, sample data, and reproducibility materials for:

> **Kiln-Net: A Gated Neural Network for Detection of Brick Kilns in South Asia.**
> Usman Nazir, Usman Khalid Mian, Muhammad Usman Sohail, Murtaza Taj, Momin Uppal.
> *IEEE Journal of Selected Topics in Applied Earth Observations and Remote Sensing (J-STARS)*, 2020.
> [DOI: 10.1109/JSTARS.2020.3001020](https://ieeexplore.ieee.org/document/9115879)

Kiln-Net is a two-stage gated pipeline for locating brick kilns from high-resolution satellite imagery across South Asia:

1. **Stage 1 — Classifier.** A CNN classifies large image tiles as *kiln* / *no-kiln*; only positive tiles are forwarded to Stage 2.
2. **Stage 2 — Detector.** A YOLO-style object detector localises individual kilns inside the positive tiles.

The gating in Stage 1 keeps the detector from running on the (very large) majority of empty terrain, which is what makes nation-scale inventories feasible.

---

## Repository Layout

```
KilnNet/
├── Classifier_Training.ipynb           # Stage-1 classifier training
├── KilnNet_Classifier_Training.ipynb   # Full pipeline training (paper version)
├── Classifier_Testing.ipynb            # Stage-1 evaluation + heatmap export
├── Detector Implementation/
│   ├── YOLOv8_Custom_Dataset_Demo.ipynb  # Stage-2 detector (YOLOv8)
│   ├── data.yaml                          # Detector dataset config
│   └── detector_Implementation.txt        # Reference to the original keras-yolo3 impl.
├── Dataset/
│   ├── train/                             # Training tiles (kiln / no-kiln)
│   └── val/                               # Validation tiles
├── Visualization files/
│   ├── HeatLayerInput.ipynb               # Build the detection heatmap
│   └── heatlayer kilns 0.49 threshold.html
├── XtileYtile to LatLon/                  # MATLAB / Octave: tile-name → lat/lon
│   ├── Zoom17tiles_Zoom20 LatLonConversion.m
│   ├── Zoom20Tiles_Zoom20LatLonConversion.m
│   └── slidingwindow.m
├── KilnLocationsPunjab.kml                # Sample output: detected kilns over Punjab
├── data.zip                               # Sample dataset bundle
├── requirements.txt
└── README.md
```

---

## Data

- **Imagery.** Google Maps tiles at zoom-17 and zoom-20. The `XtileYtile to LatLon` MATLAB/Octave scripts convert between tile coordinates and lat/lon, including a sliding-window crop from zoom-17 into zoom-20-equivalent patches.
- **Labels.** Per-tile binary labels for the classifier; bounding boxes (in YOLO format) for the detector. A small sample is included in `Dataset/` and `data.zip`.
- **Output.** `KilnLocationsPunjab.kml` and the HTML heatmap show end-to-end results: the locations of brick kilns detected across Punjab.

> The full continental-scale dataset is not redistributed in this repo. Contact the lab for access or to discuss collaborations.

---

## Reproducing the Results

### 1. Environment

```bash
python -m venv .venv
source .venv/bin/activate          # macOS / Linux
.venv\Scripts\activate             # Windows
pip install -r requirements.txt
```

The MATLAB / Octave scripts under `XtileYtile to LatLon/` are run separately (Octave ≥ 5 is sufficient — no toolboxes required beyond `image`).

### 2. Train the classifier (Stage 1)

```bash
jupyter notebook Classifier_Training.ipynb
```

Point the notebook at `Dataset/train` and `Dataset/val`. The full paper configuration is in `KilnNet_Classifier_Training.ipynb`.

### 3. Train the detector (Stage 2)

```bash
jupyter notebook "Detector Implementation/YOLOv8_Custom_Dataset_Demo.ipynb"
```

Edit `Detector Implementation/data.yaml` so `train:` / `val:` point at your YOLO-format kiln dataset and the class name is set to `kiln`.

> The original paper used [`qqwweee/keras-yolo3`](https://github.com/qqwweee/keras-yolo3) (see `detector_Implementation.txt`). The notebook here ships a modern YOLOv8 reimplementation that is easier to set up and reproduces comparable detection quality.

### 4. Build the heatmap

```bash
jupyter notebook "Visualization files/HeatLayerInput.ipynb"
```

This produces an HTML heatmap of detections (e.g. `heatlayer kilns 0.49 threshold.html`).

---

## Citation

```bibtex
@article{nazir2020kilnnet,
  author  = {Nazir, Usman and Mian, Usman Khalid and Sohail, Muhammad Usman and Taj, Murtaza and Uppal, Momin},
  title   = {Kiln-Net: A Gated Neural Network for Detection of Brick Kilns in South Asia},
  journal = {IEEE Journal of Selected Topics in Applied Earth Observations and Remote Sensing},
  volume  = {13},
  pages   = {3251--3262},
  year    = {2020},
  doi     = {10.1109/JSTARS.2020.3001020}
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
