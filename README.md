# EEC 174AY Lab 2: Supervised Learning

This lab classifies ventilator breaths as non-PVA, breath stacking
asynchrony (BSA), or double trigger asynchrony (DTA).

## Files

- `Lab2_jli.ipynb`: Main lab notebook, including code and saved outputs.
- `REPORT_QUESTIONS.md`: Written report questions and answers.
- `requirements.txt`: Python dependencies.

## Setup

Use Python 3.11

Install the dependencies:

```bash
python -m pip install -r requirements.txt
```

## Required data

Download the following datasets into the same folder as `Lab2_jli.ipynb`:

- `ece174_pva_train_x.csv`
- `ece174_pva_train_y.csv`
- `ece174_pva_validation_x.csv`
- `ece174_pva_validation_y.csv`
- `data.zip`

## Running the notebook

Start Jupyter from the repository folder:

```bash
jupyter notebook
```

Open `Lab2_jli.ipynb` and run its cells from top to bottom.

## Troubleshooting

- **FileNotFoundError:** Check that the four dataset CSV files are beside
  the notebook.
- **IndexError at `data_files[0]`:** Check that raw CSV files exist under
  `data/<patient number>/`.
- **ImportError for `simps`:** Install the dependencies from
  `requirements.txt`.