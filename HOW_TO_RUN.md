# How to Run This Study Case

This section describes how to reproduce the analysis locally.

---

## 1. Expected Repository Structure
```text
.
├── data/
│   └── public/
│       └── sc03/
│           └── v_models_analysis_o_1000.csv
├── notebooks/
│   └── sc03_chemistry_generalization_across_systems.ipynb
├── requirements.txt
└── requirements-notebooks.txt
```

---

## 2. Environment Setup

**Create a Virtual Environment**

From the repository root:
```bash
python -m venv .venv
source .venv/bin/activate
```

> **Tested with:** Python 3.10, 3.11  
> **Minimum:** Python 3.10

---

## 3. Install Dependencies

### Option A — Notebook reproduction (recommended)
```bash
pip install -r requirements-notebooks.txt
```

This installs:
- core scientific dependencies
- Jupyter execution support
- the shared portfolio toolkit

### Option B — Runtime-only (no notebooks)
```bash
pip install -r requirements.txt
```

---

## 4. Run the Notebook

### Interactive (JupyterLab)
```bash
jupyter lab
```

Then open: `notebooks/sc03_chemistry_generalization_across_systems.ipynb`

---

## 5. Verify Installation

**Check that the toolkit is installed correctly:**
```bash
python -c "from portfolio_analytics_toolkit.cv import build_oof_predictions; print('✓ Toolkit installed correctly')"
```

**Expected output:**
```
✓ Toolkit installed correctly
```

---

## 6. Quick Checks

Confirm the public dataset is present:
```bash
ls -lh data/public/sc03/v_models_analysis_o_1000
```

If files are missing, verify that you cloned the repository correctly.