# MODALES — NOx Emission Cost Comparison: Simulator vs. Mathematical Model

This folder contains MATLAB / Octave code for a NOx emission analysis carried out for the **MODALES** project (MODifying transport behaviour to reduce ALL Emission Sources, EU H2020).

The script `NOxLowDiagUppDiag.m` compares pairwise NOx emission "cost" matrices produced by two methods:

- A vehicle-emission **simulator** (`nox.csv`)
- A **mathematical model** estimate (`Noxx.csv`)

…and visualises the agreement separately for the **upper-triangular** part of the matrix (transitions corresponding to *speeding up*) and the **lower-triangular** part (transitions corresponding to *slowing down*). The included `DataFiles/AdjacencyMatrix.csv` is a 14-state speed-band adjacency matrix used to define which speed-bin transitions are admissible (a banded structure: each speed bin connects to its near neighbours only).

---

## Repository Layout

```
MODALES/
├── NOxLowDiagUppDiag.m      # Plot simulator-vs-model NOx (upper / lower diag)
├── DataFiles/
│   └── AdjacencyMatrix.csv  # 14×14 speed-band adjacency
└── README.md
```

---

## Inputs Expected by the Script

`NOxLowDiagUppDiag.m` looks for two CSV files in the **current working directory**:

| File        | Source                          | Shape  |
|-------------|---------------------------------|--------|
| `nox.csv`   | NOx cost from the simulator      | `N×N`  |
| `Noxx.csv`  | NOx cost from the math. model    | `N×N`  |

Both must be square matrices of the same size (the analysis pairs cell `(i,j)` of one against `(i,j)` of the other). `DataFiles/AdjacencyMatrix.csv` (14×14) is the reference adjacency the simulator and model are based on.

> The raw simulator / model CSVs are **not** redistributed in this repository. Contact the lab if you need them for a reproduction.

---

## Running

### Octave (free)

```bash
cd MODALES
octave --no-gui NOxLowDiagUppDiag.m
```

### MATLAB

```matlab
cd MODALES
NOxLowDiagUppDiag
```

The script produces two figures:

1. **Upper diagonal** — simulator vs. model NOx for *speeding-up* transitions.
2. **Lower diagonal** — simulator vs. model NOx for *slowing-down* transitions.

A point is plotted only when both the simulator and the model report a non-zero cost for that `(i,j)` pair, so the scatter shows the active part of the speed-bin adjacency.

---

## Project Context

MODALES (MODifying transport behaviour to reduce ALL Emission Sources) is an EU H2020 project on reducing transport-related emissions. Code in this folder supported the lab's contribution to the project's emission-modelling work-package. Project info: <https://modales-project.eu/>.

---

## Contact

- **Lab:** [Planetary Health Informatics Lab](https://www.ndorms.ox.ac.uk/research/research-groups/planetary-health-informatics-1), NDORMS, University of Oxford
- **Head of Lab:** [Dr Sara Khalid](https://www.ndorms.ox.ac.uk/team/sara-khalid)
- **GitHub:** [@phi-research](https://github.com/phi-research)

---

## License

MIT — see the top-level [LICENSE](../LICENSE).
