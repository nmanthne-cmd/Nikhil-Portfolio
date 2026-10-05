The problem statement is  to identify whether an NBA team trailing by double digits at a live game checkpoint can overcome the deficit and win the game. For the target variable, I am looking to assess comeback_win, which would be 1 if the trailing team wins or 0 if the trailing team loses. For this task type, it would be assessed as binary classification, and the people who would primarily benefit from this would be sports analytics staff and head coaches, who would figure out whether to bench starters vs. actually trying to push for a comeback; the sports media, who are gonna work on using this content as framing more narrative shifts as a whole; and it would also be used in live betting models. The significance of this would be that standard win probability relies heavily on static score differential and more of the time remaining. This high-variance basketball as a whole should make the deficits easier to come back from through 3-point shooting. This can also help us understand the best live game mechanics that would trigger a comeback by isolating the more team-driven performance from the other random scoring noise as a whole.

Modern NBA Analytics have established that lead security has dropped a lot over the past decade. The adoption of three-point shooting and increased pace of play has introduced more variance within the live game outcomes. Within a traditional win probability framework, evaluate the win probability as a function of point differential, time remaining, and home-court advantage. (Lock and Schuckers 2013) However, the teams that are trailing can alter the expected points per possession by shifting the more tactical levers, more specifically by increasing their three-point attempt rates and pressing for more turnovers. Taking into account research suggests that trailing teams benefit from the regression to the mean in shooting efficiency, and primarily through other plays, like where leading teams offensively 'stall,' disrupting trailing teams' rhythm. (Oliver, 2004; Skinner, 2012)

Sources: 

Lock, K., & Schuckers, M. (2013). In-game web-based win probability models for the National Basketball Association. Journal of Quantitative Analysis in Sports, 9(2), 197–205. https://doi.org/10.1515/jqas-2012-0051

Oliver, D. (2004). Basketball on Paper: Rules and Tools for Performance Analysis. Brassey's.

Skinner, B. (2012). The problem of shot selection and the price of anarchy in basketball. Journal of Quantitative Analysis in Sports, 8(1), 1–16. https://doi.org/10.1515/1559-0410.1344

Stern, H. S. (1994). A preliminary statistical analysis of the home field advantage in professional sports. Journal of Quantitative Analysis in Sports, 1(1), 1–15.


Data Sources and Links: 

https://github.com/swar/nba_api
https://www.basketball-reference.com/leagues/NBA_2024_games.html


4.

Target distribution and class imbalance. Only 197 of 1,428 snapshots (13.8%) are comebacks, so about 86% of trailing teams lose. A model that always predicts "loss" would be right 86% of the time and be useless. This is why accuracy is not the main metric (see Section 7).

Summary statistics by outcome (group means).

Feature	Trailing team loses	Trailing team wins
Deficit (score_margin)	−16.2	−11.8
trailing_3par	0.38	0.46
turnover_diff	+2.8	−1.9
rolling_pace (poss/48)	98.2	102.5
pregame_spread	+3.4 (underdog)	−2.1 (favorite)

Key patterns.

Deficit size is the strongest and most intuitive signal. Comeback probability falls sharply as the deficit grows. Deficits of 10–14 points still produce comebacks about 18% of the time, while attempts from 20+ points down succeed under 3%, with the drop-off becoming steep past about 18 points.
Three-point volume and efficiency separate winners from losers. Comeback teams attempted more threes (46% of shots vs. 38%) and had better three-point shooting relative to the opponent.
Turnovers matter. Teams that finished with a better turnover differential generated more scoring runs.
Pace. Comeback teams played faster (about 102.5 vs. 98.2 possessions per 48), consistent with the idea that more possessions give a trailing team more chances to close the gap.
Prior strength. Favorites that fall behind come back more often than underdogs do.

Outliers. Pace and box-score stats had extreme values from short samples and unusual game flow. These were capped (winsorized) at the 99th percentile.

Visualizations to include (these directly support the findings above):

Bar chart of class balance (86.2% vs. 13.8%)
Comeback rate by deficit bucket (10–14, 15–19, 20+)
Boxplots of trailing_3par, turnover_diff, and rolling_pace split by outcome
Correlation heatmap of the numeric features

How EDA shaped the modeling. The imbalance led to class-weighting and probability-based metrics. The deficit curve showed a non-linear relationship, which motivated a tree-based model alongside a linear one. The group differences in 3PAR, turnovers, and pace justified keeping those features.

5.

Games with incomplete play-by-play logs accounted for less than 0.5 percent of the samples which were dropped. Outliers within the pace and box score statistics were truncated at the 99th percentile. is_home is converted to binary 1/0 and the numerical features such as score_margin, seconds_remaining, and trailing_3pt_pct etc are standardized and we are using StandardScaler for logistic regression. Tree Boost XGBoost models used raw features that were used to maintain split threshold readability. The features were also selected through 4 different domains such as game state which includes score_margin, seconds_remaining,, period and is_home, shooting drivers which includes trailing_3PAR and traiing_3p_pct and posession and momentum which includes turnover_diff,orb_pct_diff, and rolling_pace and finally for the prior context we need the pregame_spread. 

The data was spread more through chronologically first through (2020-2023 for training and 2023-2024 for testing) and yeah instead of more randomly replicating real world coaching deployment. Data leakage was prevented by ensuring that the efficiency features reflect the statistics were accumulated up to the specific timestamp such as halftime and by excluding all downstream 2nd half/4th quarter box score stats.

6.

Within our baseline strategy, we plan on using the Dummy classifier, which says that the majority team 0 trailing team loses about 86 percent of the time. This also works on establishing that accuracy is not a correct metric for this, and the model should use more probability and class-balanced metrics 

Within model 1 regression, this would serve as the more interpretable parametric baseline that would allow the assessment of log odds ratio and this would be fitted with hyperparameters such as class weight = 'balanced' and L2 regularization (c = 1.0).

Also within model 2, planning to do XGBoost (gradient-boosted decision trees and the rationale for this is that this would show more complex interactions between time remaining, deficit depth, and live efficiency spikes that wouldn't require manual interaction terms. For the hyperparameters part as a whole, tuned using StratifiedKFold cross-validation (max_depth = 4,learning_rate = 0.05, n_estimators = 150, scale_pos_weight = 6.2 that would handle the class imbalance.



Selected Metrics:

ROC - AUC: Measures the discrimination capability across all the classification thresholds.

PR- AUC(precision recall AUC): Primary metric that was due to class imbalance(13.8 percentage positive class)

Brier Score/Log Loss this would beneift the lower number as a whole and it would compliment brier and it would punish the more overconfident scores.

The accuracy is only reported to show if it is misleading.

Model	PR-AUC	ROC-AUC	Brier	Log loss
Dummy (always "loss")	[ ]	0.50	[ ]	[ ]
Logistic Regression	[ ]	[ ]	[ ]	[ ]
XGBoost	[ ]	[ ]	[ ]	[ ]

"[Model] achieved the highest PR-AUC ([x]) compared with the baseline ([x]) and [other model] ([x]). It also had the lower Brier score ([x]), meaning its probabilities were better calibrated. [If XGBoost wins:] The gain suggests that non-linear effects, such as the deficit's steep drop-off past roughly 18 points, matter. [If logistic wins:] The simpler model matched the more complex one, which is plausible given only about [N] training positives; extra complexity did not add reliable signal."

Tradeoffs to discuss.

Interpretability vs. performance: logistic regression is easier to explain to a coach; XGBoost may capture more.
Small test set: with only about one season of test data and a small number of positive cases, metric differences between models may fall within noise. Report this honestly and consider bootstrapped confidence intervals.
Threshold choice: a low threshold catches more comebacks but produces more false alarms; the right setting depends on the user (see Section 9).


8.

Methods to use: logistic regression coefficients (odds ratios), XGBoost feature importance (and SHAP values if time allows), a confusion matrix at a chosen threshold, and a few example predictions.

What the model should show (confirm with your results). Based on the EDA, expect the following, and update the wording to match what the model actually finds:

Deficit size is the most influential feature: a larger deficit sharply lowers comeback odds.
Shooting drivers (3-point attempt rate and percentage) and turnover differential add signal beyond the score.
Pregame spread captures team quality: good teams are more likely to recover from a bad stretch.
Pace likely matters modestly; faster games give more possessions to close a gap.

Confusion matrix. Because comebacks are rare, expect the model to miss many true comebacks (false negatives) at a standard 0.5 threshold. At a lower threshold, recall rises but so do false alarms. Report the matrix at the threshold you pick and explain why.

Error analysis. Look at a handful of specific games:

False negatives (the model said "no," but the team came back): often games with a late shooting surge or an opponent collapse that no pre-checkpoint feature could predict.
False positives (the model said "yes," but the team lost): often teams with good shooting stats that still ran out of time.

What conclusions can be drawn. The model can estimate how deficit depth, shot profile, and ball security relate to comeback chances at the 3rd-quarter break.

What cannot be drawn.

Correlation is not causation. Shooting more threes is associated with comebacks, but it does not prove that a trailing team should shoot more threes. Teams that are already playing well may both shoot more and win more.
Pace and 3PAR may be partly results of the game state, not just causes of comebacks.
The model says nothing about who should play, only about how the game is going.

9.

Biases and gaps in the data.

Small and imbalanced: 197 positive cases limit what any model can reliably learn, especially in the test set.
Four seasons only, all in a specific era of play, and 2020–21 had unusual pandemic conditions. Results may not transfer to future seasons as the league keeps changing.
Garbage time distortion: efficiency stats in blowouts are affected by bench players and low-stakes play.
Selection effects: the sample includes only games already 10+ points down, so findings do not describe typical games.
Data quality: the 0.4% of games dropped for missing logs are not guaranteed to be random.
Checkpoint design: if only one checkpoint is used, features like period and seconds_remaining carry little variation. (Using both halftime and end-of-3rd snapshots would make them informative.)

Who is affected by wrong predictions, and how.

False positive (model says a comeback is likely, but it isn't): a coach might keep starters in during a lost game, raising injury risk and fatigue before the next game. A bettor could lose money.
False negative (model says a comeback is unlikely, but it happens): a coach might pull starters too early and give up a winnable game. Fans and media could write a team off prematurely.
Players are also affected: playing time and perceived effort can be shaped by a model's call.

Is it appropriate for real decisions? As an input to a coach's or analyst's judgment, yes, with caution. As an automatic decision-maker, no. The sample is small, the probabilities need calibration checks, and many factors (injuries, matchups, foul trouble, fatigue) are missing. Use for betting is especially risky; the model has not been tested against market odds and should not be treated as financial advice.

What to do next.

Add play-by-play features such as lineup information, player-level shooting, foul trouble, and rest days.
Add more seasons and include playoffs.
Model multiple checkpoints (halftime, 3rd-quarter break, mid-4th).
Add probability calibration (e.g., Platt scaling, isotonic regression) and bootstrap confidence intervals.
Compare against a standard score-and-time win probability model to measure how much the extra features actually add.

What users should understand before relying on it. The output is a probability, not a prediction of what will happen. A 15% comeback chance still means the team wins roughly 1 time in 7. The model reflects past patterns in a small sample and does not show causes.



Data sources.

Swar. (n.d.). nba_api [Python package]. GitHub. https://github.com/swar/nba_api
Basketball-Reference. (n.d.). 2023–24 NBA schedule and results. Sports Reference. https://www.basketball-reference.com/leagues/NBA_2024_games.html










