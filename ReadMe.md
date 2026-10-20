## Overview
This project analyzes the results of an A/B test for the mobile game *Cookie Cats*. The goal is to evaluate the impact of moving the first player gate (a forced pause in progression) from level 30 to level 40. 

## Questions to Answer
1. Does moving the gate to level 40 influence player retention (Day 1 and Day 7)?
2. Does the gate placement impact the total number of game rounds played by a user?

## Key Findings
- Day 7 retention is lower with the gate at level 40: 18.11% against 18.95% (−0.83 percentage points, 95% CI −1.34 to −0.32, p = 0.0014).
- Day 1 retention shows no clear difference: 44.17% against 44.77% (−0.60 pp, 95% CI −1.25 to +0.05, p = 0.07).
- The number of game rounds played does not differ: the 95% bootstrap CI for the difference in means includes 0.
- Recommendation: keep the gate at level 30. There is no revenue data, so the decision is based on retention and engagement only.

## Tools Used
- Python

# Analysis

## 1. Data and groups
- No missing values or duplicate user IDs.
- 91 users (0.10%) with the highest numbers of game rounds (at or above the 99.9th percentile) were removed as outliers. The results below use the remaining users.
- Group sizes before the removal: gate_30 44,700 users, gate_40 45,489 users.

![Group](assets/Group.jpg)

## 2. Hypothesis Testing

**Null hypothesis:** there is no difference in retention and in the number of game rounds between `gate_30` and `gate_40`. Significance level: α = 0.05.

### Retention Analysis (Chi-Square Test)
Since retention is a categorical/binary metric (Returned: Yes/No), the Chi-Square test of independence was applied.
- **Day 1 Retention:** $p\text{-value} = 0.07$. The difference is not statistically significant.
- **Day 7 Retention:** $p\text{-value} = 0.0014$. The difference is statistically significant, with "gate_30" outperforming "gate_40".

### Engagement Analysis (Bootstrap for Game Rounds)
Since the `sum_gamerounds` metric does not follow a normal distribution, a Bootstrap method (10,000 iterations with replacement) was used to construct a 95% Confidence Interval for the difference in means.

*Distribution before Bootstrap:*

![Not_normal](assets/before_bootstrap.png)

*Bootstrap distribution of the difference in mean rounds (gate_30 - gate_40):*

![Not_normal](assets/after_bootstrap.png)

- **Result:** The 95% Confidence Interval for the difference in means includes 0. 
- **Conclusion:** The difference in the number of game rounds played between the two groups is not statistically significant.

## 3. Recommendation
- Keep the gate at level 30: level 40 lowers Day 7 retention and does not increase the rounds played.
- The data do not show why the later gate lowers retention.
- Revenue and ad data are not in the dataset. If the gate also affects monetisation, the decision needs that data.

# Data

## Definitions
- `version`: gate_30 (control) or gate_40 (test).
- `sum_gamerounds`: rounds played.
- `retention_1`, `retention_7`: whether the player came back 1 and 7 days after installation.

## Limitations
- Group sizes are 44,700 and 45,489. If the split was meant to be 50/50, the difference is larger than expected by chance (chi-square p ≈ 0.009). The cause is not visible in the data.
- Assignment to groups is assumed to be random, as in the dataset description.
- Many players quit after a few rounds and never reach either gate. Including them dilutes any effect of the gate position. This analysis does not separate players who reached the gate.
