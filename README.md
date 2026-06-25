# Guided Growth BO Tutorial

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_GITHUB_USERNAME/Guided_growth_BO_tutorial/blob/main/bayesian_optimization_device_growth.ipynb)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

A hands-on, self-contained tutorial on **Bayesian optimization (BO) for device growth** — no machine-learning background required. We use a toy microring laser growth problem as the running example, but the workflow applies to any epitaxial growth process where experiments are expensive and the recipe space is large.

---

## What you will learn

| Section | Topic |
|---|---|
| §1 | What is Bayesian optimization? (plain language) |
| §2 | The toy growth problem — knobs, metrics, and trade-offs |
| §3 | Design of Experiments — Latin Hypercube sampling as a warm start |
| §4 | Honegumi — generating a BO template in one click |
| §5 | Single-objective BO — maximize PL intensity |
| §6 | Multi-objective BO — brightness vs a target emission wavelength |
| §7 | From the toy to real devices — the OMS-lab `Microring_MOBO` pipeline |
| §8 | Under the hood — BoTorch: GP fitting and acquisition functions |
| §9 | Recap and next steps |

---

## How to run

### Option 1 — Google Colab (recommended, nothing to install)

Click the badge at the top of this page, or open this link directly:

```
https://colab.research.google.com/github/YOUR_GITHUB_USERNAME/Guided_growth_BO_tutorial/blob/main/bayesian_optimization_device_growth.ipynb
```

Once it opens:
1. **Runtime → Change runtime type → CPU** (no GPU needed)
2. Run the **Setup cell** (Section 0)
3. **Runtime → Restart session** (once — required because we pin NumPy and pandas)
4. **Runtime → Run all**

Each person who opens the link gets their own private copy — nothing they change affects anyone else.

### Option 2 — local

```bash
git clone https://github.com/YOUR_GITHUB_USERNAME/Guided_growth_BO_tutorial.git
cd Guided_growth_BO_tutorial
pip install "ax-platform==0.5.0" "botorch==0.13.0" "numpy<2" "pandas<2.3"
jupyter notebook bayesian_optimization_device_growth.ipynb
```

> **Why the version pins?**
> `pandas>=2.3` enables Copy-on-Write which crashes Ax's internal winsorization step.
> `numpy>=2` breaks the torch/BoTorch interop. The pins are about correctness, not just reproducibility.

---

## Repository structure

```
Guided_growth_BO_tutorial/
├── bayesian_optimization_device_growth.ipynb   # main tutorial notebook
└── README.md
```

---

## The toy problem

We simulate a microring laser growth process with three knobs:

- **Growth temperature** (480–560 °C)
- **V/III precursor ratio** (5–50)
- **Ring radius** (2–10 µm)

The simulator returns two metrics — PL intensity (brightness) and emission wavelength — with a small amount of noise to mimic real device-to-device variation. The two objectives genuinely conflict: the brightest recipe emits near 1348 nm, while hitting the O-band target (1310 nm) requires a different recipe. This makes the multi-objective section non-trivial.

---

## Real pipeline

This tutorial is a miniature of the OMS-lab study on bottom-up grown InP/InAsP MQW microring lasers:

**[github.com/OMS-lab/Microring_MOBO](https://github.com/OMS-lab/Microring_MOBO)**

That repo applies the same DoE → BO workflow — with `qNParEGO` acquisition and PCA-enhanced DoE — to real MOVPE growth data. See also: Athavale et al., *ACS Photonics*.

---

## Generate your own BO template

[**honegumi.readthedocs.io**](https://honegumi.readthedocs.io) — toggle options in a grid and get a unit-tested Ax script. You change only the parameters, objectives, and the function that runs your experiment.

---

## Dependencies

| Package | Version | Role |
|---|---|---|
| `ax-platform` | 0.5.0 | BO experiment management (wraps BoTorch) |
| `botorch` | 0.13.0 | GP models and acquisition functions |
| `torch` | ≥2.2 | Tensor backend for BoTorch |
| `scipy` | any | Latin Hypercube sampling (`qmc`) |
| `numpy` | <2 | Numerical arrays (pinned for Ax compat) |
| `pandas` | <2.3 | Data frames (pinned for Ax compat) |
| `matplotlib` | any | Plots |

---

## License

MIT — see [LICENSE](LICENSE).

---

## Citation

If you use this tutorial in your work, please cite:

```bibtex
@misc{guided_growth_bo_tutorial,
  author    = {Athavale, Mihir},
  title     = {Guided Growth BO Tutorial},
  year      = {2025},
  publisher = {GitHub},
  url       = {https://github.com/YOUR_GITHUB_USERNAME/Guided_growth_BO_tutorial}
}
```
