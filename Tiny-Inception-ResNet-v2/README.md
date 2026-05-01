# Tiny-Inception-ResNet-v2 — Detecting Brick Kilns in South Asia

This folder contains the code accompanying:

> **Tiny-Inception-ResNet-v2: Using Deep Learning for Eliminating Bonded Labors of Brick Kilns in South Asia.**
> Usman Nazir, Numan Khurshid, Muhammad Ahmed Bhimra, Murtaza Taj. arXiv preprint (2019).
> [arXiv:1907.05552](https://arxiv.org/abs/1907.05552)

The paper proposes a compact Inception-ResNet-v2 variant (Block35×10, Block17×3, Block8×3 — hence the "10-3-3" suffix) for classifying high-resolution satellite tiles as brick kiln vs. non-kiln across South Asia. Reducing the number of inception-residual blocks shrinks the parameter count and inference cost so the classifier can scan large geographic areas tractably; the model is trained as a binary classifier on Zoom-20 Google Maps tiles.

---

## Repository Layout

```
Tiny-Inception-ResNet-v2/
├── inception_resnet_v2_10_3_3.py     # Tiny architecture (Keras)
├── InceptionResNet-v2-10-3-3.ipynb   # Training / fine-tuning notebook
├── InceptionResNet-v2-10-3-3.csv     # Training-curve log
├── OriginalInceptionResNet-v2.ipynb  # Baseline: full Inception-ResNet-v2
├── TinyInceptionResNet-10-3-3-Threshold-0.1.html  # Inference visualisation
├── requirements.txt
└── README.md
```

---

## Model

The Tiny variant differs from the canonical Inception-ResNet-v2 only in block depth:

| Block          | Original | Tiny ("10-3-3") |
|----------------|----------|-----------------|
| Inception-ResNet-A (Block35) | 10       | 10              |
| Inception-ResNet-B (Block17) | 20       | 3               |
| Inception-ResNet-C (Block8)  | 10       | 3               |

The reduction is applied directly in `MyInceptionResNetV2(...)` inside `inception_resnet_v2_10_3_3.py`. ImageNet-pretrained weights from the upstream Keras port are loaded as a starting point and then fine-tuned on the brick-kiln dataset.

---

## Reproducing the Results

### 1. Environment

The code targets the **Keras 2.2 / TensorFlow 1.x** API in use at the time of publication (the script imports `keras.engine.topology` and `keras_applications`, both of which were removed in modern TF/Keras). The pinned versions in `requirements.txt` reflect that.

```bash
python -m venv .venv
source .venv/bin/activate          # macOS / Linux
.venv\Scripts\activate             # Windows
pip install -r requirements.txt
```

> ⚠️ **Modern Keras compatibility:** running this against TF ≥ 2.4 / Keras ≥ 2.4 will fail at import time. Use the pinned versions, or port the imports to `tensorflow.keras` if you only need the architecture.

### 2. Train / fine-tune

Open `InceptionResNet-v2-10-3-3.ipynb`, point the data loaders at your kiln/non-kiln tile folders, and run all cells. Per-epoch metrics are appended to `InceptionResNet-v2-10-3-3.csv`.

### 3. Inspect predictions

`TinyInceptionResNet-10-3-3-Threshold-0.1.html` shows the model's predictions on a held-out region at a 0.1 confidence threshold.

---

## Citation

```bibtex
@article{nazir2019tiny,
  title   = {Tiny-Inception-ResNet-v2: Using Deep Learning for Eliminating Bonded Labors of Brick Kilns in South Asia},
  author  = {Nazir, Usman and Khurshid, Numan and Bhimra, Muhammad Ahmed and Taj, Murtaza},
  journal = {arXiv preprint arXiv:1907.05552},
  year    = {2019}
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
