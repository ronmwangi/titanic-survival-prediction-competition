**# Titanic Survival Prediction (Kaggle)**



Predicting which passengers survived the Titanic using feature engineering and

gradient boosting, validated with repeated cross-validation.



\*\*Result:\*\* 0.839 cross-validated accuracy, \*\*0.770 on the Kaggle public leaderboard\*\*

(baseline gender rule: 0.766).



\## The problem

Given passenger details (class, sex, age, family, fare, and so on), predict survival

(1 = survived, 0 = died). 891 labeled passengers for training, 418 for testing.

Metric: accuracy.



\## Approach

1\. \*\*Exploratory analysis.\*\* Sex, class, and title were the strongest signals. Family

&#x20;  size had a "sweet spot": passengers with 1 to 3 relatives did best, while large

&#x20;  families (5+) did worst. The Embarked effect was mostly class in disguise (51% of

&#x20;  Cherbourg passengers were first class).

2\. \*\*Cleaning.\*\* Missing ages were filled with the median age for each title, so a

&#x20;  "Master" (young boy) gets about 3.5 and a "Mrs" gets about 35. All fill values were

&#x20;  calculated on training data only, so no test information leaked in.

3\. \*\*Feature engineering.\*\* Title (from names), family size groups

&#x20;  (Alone / Small / Large), and a has-cabin flag.

4\. \*\*Modeling.\*\* Compared Logistic Regression, Random Forest, and Gradient Boosting

&#x20;  using 5-fold cross-validation repeated 3 times.



\## Results

| # | Experiment | CV accuracy | Leaderboard | Decision |

|---|-----------|-------------|-------------|----------|

| 1 | Gender rule baseline | 0.787 | 0.76555 | Baseline |

| 2 | Gradient Boosting + Title, FamilyGroup, HasCabin | 0.8387 | 0.77033 | \*\*Final model\*\* |

| 3 | + TicketCount / FarePerPerson | 0.834 to 0.837 | not submitted | Dropped (no gain) |

| 4 | Randomized hyperparameter search | 0.835 vs 0.832 (fresh splits) | not submitted | Dropped (inside noise) |

| 5 | Soft-voting ensemble | 0.8305 / 0.8331 | not submitted | Dropped (inconsistent) |



!\[Feature importance](feature\_importance.png)



`Title\_Mr` carries the most weight. Note that `Sex` looks small only because the title

already encodes it, and tree-based importance favors continuous columns like Fare.

The chart shows what this model used, not a ranking of real-world causes.



\## What I learned

\- Title and family-size features gave the real gain. Tuning and ensembling did not.

\- Fold-to-fold scores varied by about +/-0.018, so I only kept changes that beat that

&#x20; noise. With 418 test passengers, one passenger is worth about 0.24 points on the

&#x20; leaderboard, so small leaderboard moves are mostly luck.

\- Cross-validation (0.839) is higher than the leaderboard (0.770). Part of that is a

&#x20; small, noisy test set. I made decisions with cross-validation and used the

&#x20; leaderboard only as a final check.



\## Limitations and next steps

\- The model has no information about where on the ship each passenger was, or who

&#x20; helped whom, so there is a ceiling on honest accuracy for this dataset.

\- Next steps: look at the passengers the model gets wrong, test cabin-deck features,

&#x20; and try the same pipeline on a larger dataset.



\## Run it

Data: Titanic Survival Prediction

(https://www.kaggle.com/competitions/titanic).



Open `notebooka80a5868dd.ipynb` in a Kaggle notebook with the competition data attached.



\*\*Author:\*\* Ron Mwangi | https://www.linkedin.com/in/ron-ngugi-8847b127a/

