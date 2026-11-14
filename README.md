# DSCI 552 Homework 7

## Repository contents

This repository contains three files:

- `DSCI552_HW7_Report.pdf`: the report (7 pages) with code excerpts, tables, the CH curve, the Monte Carlo results, and the ISLR 12.6.2 dendrograms.
- `DSCI552_HW7.ipynb`: the companion notebook with all code and saved outputs. It covers the required questions 1(a), 1(b)(i)–(iv), 2(a)–(c), and 3. Extra Practice items 1(b)(v), 1(b)(vi), and 4 are not included.
- `README.md`: this file.

## Reproduce the results

### 1. Get the course data

The data archive `Homework 7 Data.zip` is not in this repository. Download it from the DSCI 552 course materials and keep its original name. The notebook reads `Anuran Calls (MFCCs)/Frogs_MFCCs.csv` directly from the ZIP, so it does not need to be unzipped.

### 2. Install the dependencies

The recorded run used Python 3.13.5 with these package versions:

```text
numpy==2.4.3
pandas==3.0.1
scipy==1.17.1
scikit-learn==1.8.0
imbalanced-learn==0.14.1
joblib==1.5.3
threadpoolctl==3.6.0
python-dateutil==4.0.0
sklearn-compat==0.1.6
tzdata==2026.6
matplotlib==3.11.2
ipykernel==7.4.0
nbconvert==7.17.2
```

Create an environment and install them:

```sh
python3.13 -m venv .venv
. .venv/bin/activate
python -m pip install \
  numpy==2.4.3 \
  pandas==3.0.1 \
  scipy==1.17.1 \
  scikit-learn==1.8.0 \
  imbalanced-learn==0.14.1 \
  joblib==1.5.3 \
  threadpoolctl==3.6.0 \
  python-dateutil==4.0.0 \
  sklearn-compat==0.1.6 \
  tzdata==2026.6 \
  matplotlib==3.11.2 \
  ipykernel==7.4.0 \
  nbconvert==7.17.2
```

To use Run All in a browser, also install a Jupyter front end such as JupyterLab. The command-line route below needs only the packages above.

### 3. Place the files

Put `DSCI552_HW7.ipynb` and `Homework 7 Data.zip` in the same empty directory:

```text
hw7/
├── DSCI552_HW7.ipynb
└── Homework 7 Data.zip
```

### 4. Run the notebook

With the environment activated, run all cells from that directory, either with Run All in a Python 3 kernel or with:

```sh
python -m nbconvert --to notebook --execute --ExecutePreprocessor.timeout=5400 \
  --output DSCI552_HW7_reexecuted.ipynb DSCI552_HW7.ipynb
```

The full run fits all SVM searches and the 50 K-means runs, so it takes a long time. The setup cell uses `JOBS = 8`; lower it on a machine with fewer cores. Seeds are fixed (split 552, CV 553, SMOTE 554, linear solver 555, clustering 7001–7050). Different library builds or platforms may give small numerical differences.

The run writes fresh predictions, CV scores, CH scores, and cluster assignments to `notebook_results/`. The metric-review cells at the end of the notebook read that directory. If those cells are run on their own without these outputs, an input-check cell stops with a message telling you to run the experiment cells first.

## About the saved notebook outputs

The training and clustering outputs come from an earlier full execution in a clean directory with the course ZIP. The final metric-review cells (22-class binary Hamming loss and sample-averaged Jaccard accuracy) were executed later in a separate session against the author's local `results/` archive from that full run. The models and partitions were not refit. The notebook explains this in the section "Q1 and Q2 metric review from saved predictions".

## Files kept locally, not in this repository

The following files were used to produce the report but are not needed to reproduce the results from the notebook: `hw7_experiments.py` (script version of the notebook code), `build_hw7_report.py`, `DSCI552_HW7_Report.tex`, `hw7_discussion.tex` (report build files), `requirements.txt` (its pinned versions are listed above), and `results/` (cached outputs of the original run).

## Metric definitions

The three-slot Hamming distance, loss, and score treat Family, Genus, and Species as three label positions. The three-slot Hamming score is the fraction of those positions predicted correctly. The 22-class binary-indicator Hamming loss and the sample-averaged Jaccard accuracy follow Sorower §7.1.1. The Jaccard accuracy is the intersection over union of each row's true and predicted label sets, averaged over rows.

## AI Usage Disclosure

I used AI tools to help me learn the relevant programming techniques and machine learning concepts, understand code implementations, and assist with debugging.
