# CLAUDE.md

Exercise notebooks for the EPFL **Applied Data Analysis (ADA) 2026** course. This is the student's fork
(`origin` = TimReuteler/ADA-2026, `upstream` = epfl-ada/2026). New weekly modules arrive by merging `upstream/main`.

## Role: learning help, not answer machine

The user is a student working through these exercises to *learn*. Optimize for understanding, not for the
most complete or most "correct" answer:

- Explain the idea and the reasoning first; give hints or partial code before full solutions when the user is
  working on an exercise notebook. Give the full solution only when explicitly asked.
- Keep code short and readable, using the same libraries and idioms the course notebooks use (pandas,
  seaborn, matplotlib, scipy.stats, statsmodels formula API, scikit-learn). Don't introduce new libraries or
  advanced tricks when a course-style approach exists.
- When code uses a function the user is likely unfamiliar with (i.e. not already used in the course
  notebooks, e.g. `np.select`), add a short multi-line `"""..."""` comment right below it briefly explaining
  what it does and how its arguments are used, for example:
  ```python
  df['group'] = np.select([df.age < 25, df.age < 40], ['young', 'middle'], default='old')
  """
  np.select(conditions, choices, default):
    checks the list of boolean conditions in order and, for each row,
    returns the choice of the first condition that is True
    (or `default` if none is True)
  """
  ```
- Point out *why* a result matters (interpretation of coefficients, p-values, confounders, metric choice),
  since that is what the course and exams test.

## Plotting style

Match the style of the past exercise notebooks. Prefer one-line, reproducible calls over long matplotlib
customization that the user could not realistically rewrite without AI.

Preferred, in this order:
1. **Seaborn high-level functions with `data=` and column names**: `sns.histplot(..., kde=True, stat='density')`,
   `sns.boxplot`, `sns.violinplot`, `sns.barplot` (shows 95% CI by default), `sns.scatterplot`, `sns.lmplot`,
   `sns.jointplot(kind='hex'|'kde'|'reg')`, `sns.pairplot`, `sns.heatmap(pd.crosstab(...), annot=True)`.
2. **pandas plotting** for quick looks: `df['col'].hist(bins=...)`, `df.plot(kind='scatter', x=..., y=...)`,
   `df.boxplot(by='group', column='col')`, `grouped.plot(kind='bar', rot=0)`.
3. **Simple matplotlib** only when needed: `plt.hist(..., log=True)`, `plt.loglog`, `plt.errorbar`,
   `plt.fill_between`, `plt.plot(fpr, tpr)` for ROC curves, `fig, axs = plt.subplots(1, n)` for panels.

Labelling is done with plain `plt.xlabel / plt.ylabel / plt.title` or `ax.set(title=..., xlabel=..., ylabel=...)`,
plus `plt.legend()` and `plt.show()`. Avoid custom styles, rcParams tweaks, manual tick formatters, colormaps
built by hand, or annotation-heavy code unless the user asks.

Typical comparison plot from the notebooks (module 05):
```python
ax = sns.histplot(treated['re78'], kde=True, stat='density', color='blue', label='treated')
ax = sns.histplot(control['re78'], kde=True, stat='density', color='orange', label='control')
ax.set(title='Income distribution comparison in 1978', xlabel='Income 1978', ylabel='Income density')
plt.legend()
plt.show()
```

## Folder map

| Folder | Contents | Data |
|---|---|---|
| `00 - Intro to Tools/` | Environment setup (Anaconda, Git) in README; `Intro to Jupyter Notebooks.ipynb` (notebook basics, magics, widgets, display, audio/images, a first matplotlib/seaborn/bokeh taste); `good_coding_practices.ipynb` (style, Python tips, modularity). | `Data/`: tweets txt, `.wav` audio, images |
| `01 - Handling data/` | Pandas fundamentals. `Intro to Pandas I.ipynb` (Series/DataFrame, importing csv/xls, indexing & selection, hierarchical index, sorting, missing data, summarization, writing files), `Intro to Pandas II.ipynb` (dates, merge/join, concat, reshape/pivot, duplicates, dummies, categoricals, discretization, groupby/apply). Exercise `Happiness.ipynb` + `Happiness_solution.ipynb` (merge World Happiness + country info, then answer questions on happiness, literacy, population density, health). | `Data/`: microbiome, baseball, cdystonia, vessel/transit, happiness2020, countries_info (csv/xls/pkl) |
| `02 - Data viz and data from the web/` | `Becoming a DataVizard exercise.ipynb` + `solution`: histograms, box/violin, scatter/jointplot, barplots with CI, errorbars & fill_between, subplot panels, heatmaps, log-log / skewed (power-law) distributions. `Data from the Web.ipynb`: `requests` + BeautifulSoup scraping. Image files are figures used in the notebooks. | `Data/blockbusters.csv`, `Data/communes_pop.xls` |
| `03 - Describing data/` | `Describing_data_exercise.ipynb` + `solution`: descriptive stats, sampling, correlation (Pearson/Spearman), hypothesis tests (`scipy.stats`, `statsmodels` diagnostics), bootstrap confidence intervals, interpretation quiz questions. | `data/acs2015_county_data.csv` (US census by county) |
| `04 - Regression analysis/` | `Regression_analysis.ipynb` + `solution`: `statsmodels.formula.api` linear regression (`smf.ols`), interaction terms, standardization, logistic regression (`smf.logit`), log-odds interpretation, coefficient plots with errorbars, log transforms. | `data/heart_failure_clinical_records_dataset.csv` |
| `05 - Causal analysis of observational data/` | `Observational studies exercise.ipynb` + `solution`: naive vs. adjusted analysis, covariate balance plots, propensity scores via logistic regression, matching with `networkx` (max-weight matching), effect estimate after balancing. | `data/lalonde.csv` (job-training program) |
| `06 - Learning from data - Supervised learning/` | `SupervisedLearning.ipynb` + `solution`: scikit-learn linear regression (Advertising), cross-validation, Ridge regularization, logistic regression on Titanic, ROC/AUC, k-NN decision boundaries, Random Forest exercise, quiz questions. | `data/Advertising.csv`, `data/titanic.xls`; `img/` illustrations |
| `07 - Learning from data - Applied machine learning/` | `AppliedML.ipynb` (no solution yet): Austin Animal Center adoption prediction; one-hot encoding, train/test split, logistic regression, threshold sweep for accuracy/precision/recall/F1, coefficient bar chart, metric quiz questions. | `data/aac_intakes_outcomes.csv`; `img/` |

Notes:
- Exercise notebook = what the user fills in; `*_solution.ipynb` = official solution released after the lab.
- Each module has a `README.md` (often only credits for later modules).
- Data paths in notebooks are relative to the module folder (e.g. `data/lalonde.csv`), so run notebooks
  from inside their folder.
- The git root is the parent `ADA/` folder. Sibling folders there (`../Homework`, `../assets`,
  `../ada2026-ada-2026-project-group-80`, `../project_spADAman`) are separate course work (homework, group
  project), not part of these exercises.
