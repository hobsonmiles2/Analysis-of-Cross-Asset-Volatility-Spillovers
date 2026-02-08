# File Path Update Guide

This guide shows the file path changes needed for notebooks 04 and 05.

## Quick Fix: Add This Cell to the Start of Each Notebook

```python
from pathlib import Path

# Define standardized paths
DATA_DIR = Path('../data/processed')
OUTPUT_DIR = Path('../outputs')
METADATA_DIR = Path('../data/metadata')

# Create output directories
(OUTPUT_DIR / 'regime_switching').mkdir(parents=True, exist_ok=True)
(OUTPUT_DIR / 'ml').mkdir(parents=True, exist_ok=True)
(OUTPUT_DIR / 'spillovers').mkdir(parents=True, exist_ok=True)
(OUTPUT_DIR / 'rolling').mkdir(parents=True, exist_ok=True)

print("✓ Paths configured")
```

---

## Notebook 04: Regime_Identification

### Find and Replace

| Old Path | New Path |
|----------|----------|
| `"all_assets_conditional_vol.csv"` | `OUTPUT_DIR / "all_assets_conditional_vol.csv"` |
| `"outputs/rolling/rolling_total_H10_W250_S5_fixed.csv"` | `OUTPUT_DIR / "rolling" / "rolling_total_H10_W250_S5_fixed.csv"` |
| `"dcc_correlations.csv"` | `OUTPUT_DIR / "dcc_correlations.csv"` |
| `out_dir = Path("outputs/regime_switching")` | `out_dir = OUTPUT_DIR / "regime_switching"` |

### Common Patterns to Update

**Before:**
```python
df = pd.read_csv("all_assets_conditional_vol.csv")
```

**After:**
```python
df = pd.read_csv(OUTPUT_DIR / "all_assets_conditional_vol.csv")
```

---

## Notebook 05: Diss_ML (Machine Learning)

### Input Files to Update

| Variable | Old | New |
|----------|-----|-----|
| `CSV_TOTAL_SPILLOVER` | `"outputs/rolling/rolling_total_H10_W250_S5_fixed.csv"` | `OUTPUT_DIR / "rolling" / "rolling_total_H10_W250_S5_fixed.csv"` |
| `CSV_NET` | `"outputs/rolling/rolling_net_H10_W250_S5_fixed.csv"` | `OUTPUT_DIR / "rolling" / "rolling_net_H10_W250_S5_fixed.csv"` |
| `CSV_VOL` | `"outputs/all_assets_conditional_vol.csv"` | `OUTPUT_DIR / "all_assets_conditional_vol.csv"` |
| `CSV_DCC` | `"outputs/dcc_correlations.csv"` | `OUTPUT_DIR / "dcc_correlations.csv"` |
| `CSV_REGIME_SEG` | `"outputs/trackC_segments.csv"` | `OUTPUT_DIR / "trackC_segments.csv"` |
| `SEGMENTS_CSV` | `"outputs/trackC_segments.csv"` | `OUTPUT_DIR / "trackC_segments.csv"` |

### Output Directory

**Before:**
```python
OUT_DIR = "outputs/ml"
```

**After:**
```python
OUT_DIR = OUTPUT_DIR / "ml"
OUT_DIR.mkdir(parents=True, exist_ok=True)
```

---

## Automated Find & Replace Script

Save this as `fix_paths.py` and run it:

```python
import json
from pathlib import Path

def fix_notebook_paths(notebook_path, replacements):
    """Fix file paths in a Jupyter notebook"""
    with open(notebook_path, 'r', encoding='utf-8') as f:
        nb = json.load(f)

    count = 0
    for cell in nb['cells']:
        if cell['cell_type'] == 'code' and 'source' in cell:
            if isinstance(cell['source'], list):
                for i, line in enumerate(cell['source']):
                    for old, new in replacements.items():
                        if old in line:
                            cell['source'][i] = line.replace(old, new)
                            count += 1
            elif isinstance(cell['source'], str):
                for old, new in replacements.items():
                    if old in cell['source']:
                        cell['source'] = cell['source'].replace(old, new)
                        count += 1

    with open(notebook_path, 'w', encoding='utf-8') as f:
        json.dump(nb, f, indent=1, ensure_ascii=False)

    return count

# Notebook 04 replacements
nb04_replacements = {
    '"all_assets_conditional_vol.csv"': 'OUTPUT_DIR / "all_assets_conditional_vol.csv"',
    '"outputs/rolling/': 'OUTPUT_DIR / "rolling" / "',
    '"dcc_correlations.csv"': 'OUTPUT_DIR / "dcc_correlations.csv"',
    'Path("outputs/regime_switching")': 'OUTPUT_DIR / "regime_switching"',
}

# Notebook 05 replacements
nb05_replacements = {
    '"outputs/rolling/': 'OUTPUT_DIR / "rolling" / "',
    '"outputs/all_assets_conditional_vol.csv"': 'OUTPUT_DIR / "all_assets_conditional_vol.csv"',
    '"outputs/dcc_correlations.csv"': 'OUTPUT_DIR / "dcc_correlations.csv"',
    '"outputs/trackC_segments.csv"': 'OUTPUT_DIR / "trackC_segments.csv"',
    'OUT_DIR = "outputs/ml"': 'OUT_DIR = OUTPUT_DIR / "ml"',
}

# Fix notebooks
count_04 = fix_notebook_paths('Notebooks/04_Regime_Identification.ipynb', nb04_replacements)
count_05 = fix_notebook_paths('Notebooks/05_Diss_ML.ipynb', nb05_replacements)

print(f"✓ Fixed {count_04} paths in Notebook 04")
print(f"✓ Fixed {count_05} paths in Notebook 05")
```

---

## Standard Path Structure (For Reference)

All notebooks should follow this pattern:

```
Notebooks/
├── 01_DataCollect.ipynb         ✓ FIXED
├── 02_GARCH_DCC_GARCH.ipynb    ✓ FIXED
├── 03_Spillover_Analysis.ipynb  ✓ FIXED
├── 04_Regime_Identification.ipynb ⚠ NEEDS UPDATE
└── 05_Diss_ML.ipynb              ⚠ NEEDS UPDATE

data/
├── raw/                  # Downloaded CSV files
├── processed/            # Cleaned datasets (all_assets_log_returns.csv, etc.)
└── metadata/             # Asset classifications

outputs/
├── spillovers/           # Static spillover analysis
├── rolling/              # Rolling window results
├── regime_switching/     # Regime identification
├── ml/                   # Machine learning outputs
└── *.csv                 # Shared outputs (DCC, conditional vol)
```

---

## Quick Manual Fix

If you prefer to manually update:

1. **Add the path setup cell** at the beginning of notebooks 04 and 05
2. **Replace hardcoded strings:**
   - `"outputs/` → `OUTPUT_DIR / "`
   - `"all_assets_conditional_vol.csv"` → `OUTPUT_DIR / "all_assets_conditional_vol.csv"`
   - `"dcc_correlations.csv"` → `OUTPUT_DIR / "dcc_correlations.csv"`

3. **Update Path() calls:**
   - `Path("outputs/...)` → `OUTPUT_DIR / "..."`

That's it! All notebooks will then use the standardized structure.
