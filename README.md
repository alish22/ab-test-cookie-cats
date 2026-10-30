# A/B Test Analysis: Cookie Cats

An analysis of a mobile game A/B test in Python. The test moved the first "gate" in the game from level 30 to level 40. I check if this changed how many players came back.

**Main result:** moving the gate to level 40 **lowered 7-day retention** from 19.02% to 18.20% (-0.82 percentage points, 95% confidence interval -1.33 to -0.31, p = 0.0031 after a Holm correction). It did not improve any measure. Recommendation: keep the gate at level 30, unless revenue data shows a clear gain from the change.

The notebook with all charts and results is here: [`ab_test_cookie_cats.ipynb`](ab_test_cookie_cats.ipynb)

## What the notebook does

1. **Data checks:** missing values, duplicate player IDs, extreme values.
2. **Sample ratio check:** a chi-square test to see if the 50/50 split was really 50/50.
3. **Retention comparison** (1-day and 7-day): rates, difference in percentage points, 95% confidence intervals, a two-proportion z-test, a bootstrap interval as a second check, a Holm correction for two metrics, and the probability that the change is worse.
4. **Game rounds** as a second view of engagement: Mann-Whitney U test, medians, and the effect of one extreme player.
5. **Power and sample size:** the smallest change this test could detect, and how many players are needed to detect smaller changes.
6. **Written recommendation and limits.**

## Results at a glance

| | gate_30 | gate_40 | Difference | Significant? |
|---|---|---|---|---|
| 1-day retention | 44.82% | 44.23% | -0.59 pp | No (p = 0.074) |
| 7-day retention | 19.02% | 18.20% | -0.82 pp | Yes (p = 0.0031, Holm) |
| Median rounds played | 17 | 16 | -1 round | Borderline (p = 0.0502) |

The test has 90,189 players. The two groups differ in size a little more than chance usually allows (p = 0.0086). I flag this as a small warning in the notebook.

## Data

The data is the public "Mobile Games A/B Testing - Cookie Cats" dataset (90,189 players, 5 columns). Search for that name on Kaggle, download `cookie_cats.csv` and put it in the `data/` folder. The data file is not stored in this repository.

| Column | Meaning |
|---|---|
| `userid` | Player ID |
| `version` | `gate_30` (control) or `gate_40` (variant) |
| `sum_gamerounds` | Rounds played in the first 14 days |
| `retention_1` | Player came back after 1 day |
| `retention_7` | Player came back after 7 days |

## How to run

```bash
pip install -r requirements.txt
jupyter notebook ab_test_cookie_cats.ipynb
```

Then choose **Run all**. It takes a few seconds.

## Limits

- The data has no revenue, so it cannot show if the later gate earns more money.
- Retention is only measured at 1 and 7 days.
- The effect is probably diluted, because players who quit before level 30 never see the gate.

## Tools

Python, pandas, NumPy, SciPy, Matplotlib, Jupyter.
