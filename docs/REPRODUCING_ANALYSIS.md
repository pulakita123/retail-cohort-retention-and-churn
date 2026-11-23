# Reproducing the Analysis

## Requirements

- Python 3.9 or later recommended
- The repository workbook: `Online Retail.xlsx`
- The dependencies listed in `requirements.txt`

## Setup

From the repository directory, create and activate a virtual environment.

**Windows PowerShell**

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

**macOS / Linux**

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Run the notebook

Start Jupyter:

```bash
jupyter notebook
```

Open `cohort_retention.ipynb` and run the cells from top to bottom. The notebook expects `Online Retail.xlsx` to be in the repository's root directory and reads the first worksheet.

The notebook creates the cohort-retention heatmap and performs the customer-level inactivity and baseline classification analysis.

## Reproducibility notes

- The notebook uses a random seed of 42 for the train-test split.
- The classification metrics may vary if package versions or source data change.
- The inactivity reference date is derived from the maximum transaction date in the filtered dataset.
- The existing notebook contains saved outputs from a prior execution. Re-run all cells to refresh outputs against the workbook in the current checkout.
- The model's current feature timing has leakage limitations; see [Methodology](METHODOLOGY.md) before interpreting its score.

## Dependencies

The project requirements include pandas, NumPy, Matplotlib, Seaborn, scikit-learn and openpyxl.
