# DSCI 552 Homework 7

Run these commands from the homework directory. The Python script reads the CSV directly from `Homework 7 Data.zip`. The supplied ZIP and assignment PDF are preserved.

## Submission files

- `DSCI552_HW7_Report.pdf` is the English report with code excerpts, the CH curve, Monte Carlo results, and the ISLR dendrograms.
- `DSCI552_HW7.ipynb` is the executed, self-contained companion. The required questions are 1(a), 1(b)(i)–(iv), 2(a)–(c), and 3. Extra Practice items 1(b)(v), 1(b)(vi), and 4 are omitted.

## Reproduce from the notebook

Install the pinned dependencies below. Place the notebook and `Homework 7 Data.zip` together in a clean directory. With the environment activated, use Run All in a Python 3 kernel or run the following from that directory.

~~~sh
python -m nbconvert --to notebook --execute --ExecutePreprocessor.timeout=5400 \
  --output DSCI552_HW7_reexecuted.ipynb DSCI552_HW7.ipynb
~~~

The notebook embeds the experiment code and performs the full fit. It creates `notebook_results/` with fresh predictions, CV scores, CH scores, and cluster assignments. Reduce `JOBS` in the setup cell if needed.

## Reproduce from the Python script

The recorded run used Python 3.13.5 and the package versions in `requirements.txt`.

```sh
python3.13 -m venv .venv
. .venv/bin/activate
python -m pip install -r requirements.txt
python hw7_experiments.py all --jobs 4
```

Use `svm` or `kmeans` in place of `all` to rerun one section. Set `--jobs 1` to run sequentially. BLAS/OpenMP thread counts are limited during fitting. The split seed is 552, CV seed is 553, SMOTE seed is 554, linear solver seed is 555, and the 50 clustering seeds are 7001 through 7050. Library builds and platforms may produce small numerical differences.

## Rebuild the report from saved results

Install TeX Live or MacTeX with `pdflatex` on PATH and the packages declared in `DSCI552_HW7_Report.tex`, including TikZ/PGF. Run the following to verify the cache and build the report without fitting the models again.

```sh
python build_hw7_report.py
pdfinfo DSCI552_HW7_Report.pdf
```

The builder reconstructs the shared holdout, verifies the nine tuning searches and test metrics, and recomputes the cluster label assignments and metrics for all 50 runs. It inserts the verified numbers into the report. If experimental settings change, review the methods in the TeX file and update the interpretation in `hw7_discussion.tex`.

## Saved results

- `results/svm_summary.json` holds the split statistics, model metrics, selected hyperparameters, and per-class metrics.
- `results/svm_test_predictions.csv` holds each test row's zero-based source index, original labels, and all nine predictions.
- `results/svm_cv.csv` holds every grid candidate's 10 fold scores.
- `results/kmeans_monte_carlo.csv` holds the 50 seeds, selected cluster counts, CH scores, and Hamming metrics.
- `results/kmeans_ch_candidates.csv` holds 2,450 candidate CH scores.
- `results/kmeans_partitions.npz` holds each run's winning cluster assignments in original CSV row order, keyed by seed.

## Reproduction check

The submitted notebook executed all eight code cells in a clean directory with the course ZIP. The rerun matched the original cache exactly for the shared split, all metrics and per-class results, selected hyperparameters, 2,159 test prediction rows, all fold scores in the 126 CV candidate rows, all 50 Monte Carlo records, all 2,450 CH candidate scores, and all 50 winning partitions. CV timing columns were excluded because they measure elapsed runtime. The report tables were checked against the same verified cache.
