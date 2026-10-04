# ENME 485/691 Industrial AI — coursework

Homework solutions for ENME 485/691 Industrial AI (Fall 2026). Each homework is a self-contained Jupyter notebook that goes from raw vibration data to a health assessment, plus the report and figures generated from it.

## Repository layout

| Path | Contents |
|---|---|
| `hw2/` | Homework 02: two-class health assessment (healthy vs. unbalanced shaft) with **logistic regression**. `hw2_rotor_bearing.ipynb` is the full analysis; `results/` holds the extracted features and per-record predictions as CSV; `report/` holds the Word report and the figures exported from the notebook. `test.ipynb` and `test2.ipynb` are scratch files. |
| `hw3/` | Homework 03: three-class assessment (healthy, unbalance level 1, unbalance level 2) with a **support vector machine**. Same structure: `hw3_rotor_bearing_svm.ipynb`, `results/`, `report/`. |
| `data/` | Raw vibration records used by the notebooks (see below). |
| `src/enme691/` | Placeholder package created by `uv init`; nothing in the homework depends on it. |
| `pyproject.toml`, `uv.lock` | Environment definition. |

Each `hwN/` folder also contains the homework handout as a PDF.

## Data

The data come from the rotor-bearing testbed described in the homework handouts and were provided by the course. A shaft with a disk runs at a nominal 20 Hz (about 21 Hz measured). Screws added to the disk create a mass unbalance: one screw is unbalance level 1, two screws are unbalance level 2. An accelerometer on the bearing block records vibration at 2560 Hz. Every file is one 15 s record of 38 400 acceleration samples in g, preceded by a short header (date, time, operator, `Acceleration`). All records were acquired on March 26, 2014; the acquisition time and a record index are encoded in each file name.

| Folder | Used by | Contents |
|---|---|---|
| `data/rotor-bearing/` | HW2 | `Training/Healthy` (20), `Training/Faulty` (20, unbalance level 2), `Testing` (30, unlabeled). |
| `data/rotor-bearing-hw3/` | HW3 | `Training/Healthy` (20), `Training/Faulty/Unbalance 1` (20), `Training/Faulty/Unbalance 2` (20), `Testing` (30). The healthy, level-2 and test files are identical to the HW2 files; the appendix of the HW3 handout gives the true class of each test record for scoring. |
| `data/gearbox/` | not yet used | Empty placeholder. |

## Setup

The project uses [uv](https://docs.astral.sh/uv/) with Python 3.13 (pinned in `.python-version`). Dependencies are numpy, scipy, pandas, scikit-learn, matplotlib, seaborn, statsmodels and Jupyter.

```bash
uv sync
```

This creates `.venv/` and installs everything from `uv.lock`. No other tools are required to run the notebooks.

## Running the notebooks

Open a notebook in Jupyter Lab:

```bash
uv run jupyter lab hw3/hw3_rotor_bearing_svm.ipynb
```

or execute one end to end from the command line, writing the outputs back into the file:

```bash
uv run jupyter nbconvert --to notebook --execute --inplace hw3/hw3_rotor_bearing_svm.ipynb
```

The notebooks locate the data relative to their own folder (`../data/...`), so they also work when launched from the repository root. Each notebook writes its feature tables and predictions to `hwN/results/`. A full run takes about a minute.

In VS Code, open the notebook and select the `.venv` interpreter as the kernel.

## Reports

The Word reports in `hwN/report/` were generated from the executed notebooks with `python-docx` (figures are exported from the notebook outputs into `report/figures/`). The generator scripts are not part of the repository; the notebook is the source of record for every number and figure in the reports.
