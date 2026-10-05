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

Within the datastructure and limitations, a single NBA game snapshot is recorded at a specific game checkpoint: halftime or the end of the 3rd quarter, where the leading team forces a deficit of 10 or more points. We will look at 4 datasets from 2020-2021 through 2023-2024, covering 4920 regular-season games. I would primarily be filtering for instances where a team trailed by 10 or more points at the 3rd-quarter break, which yielded a smaller dataset of 1428 unique comeback opportunity snapshots. My target variable would be comeback_win, and it would be binary: 0 if the trailing team loses and 1 if the trailing team wins. The primary available features that I am looking at are score_margin which would be the trailing team score - leading score), seconds remaining, period, and is_home 1 is home, and 0 is away. For the effiency and shot portfolio I will be looking at trailing_3par which would convert to the 3PT Attempts/total FG attempts, trailing_3p_pct which would calculate the 3P% to the timestamp which would be calculated as t,turnover_diff(leading turnovers - trailing turnovers), orb_pct_diff(offensive rebound % diffrential), flow and context which would measure the posessions that were calculated over preceding 6-12 minutes; and pregame_spread which would calculate the pregrame spread which would be a baseline from before. I would also have to account for API logging anomalies because stats.nba.com occasionally contains missing timestamp events, and I tracked this through 0.4 percent of the games, which were dropped during the cleaning process. Finally, I have to account for garbage time distortions, and this model does not really account for when starters are benched during blowouts, which can distort the trailing efficiencies through the 4th quarter.

Out of the 1428 double-digit trailing snapshots through the four seasons, trailing teams successfully complete a comeback in 197 games, which results in a class imbalance as a whole. Within the score_margin, the trailing team loses -16.2 points, and the trailing team wins when the deficit is -11.8 points; for the trailing 3-par, the trailing team loses 0.38, and 0.46 trailing team wins and the trailing team loses if they have a turnover differential of +2.8, and they win if they minimize the differential, which is -1.9 turnovers. For the rolling pace part of it, the trailing team would lose if they have 98.2 possessions/48 min and they win if they have 102.5 possessions/48 min; and for the pregame spread as a whole, a 3.4 underdog would lose, and if they are -2.1 and the favorite, then they have a higher chance to win.

Within this, the key EDA insights are that the comeback probability would drop exponentially past an 18-point deficit, and the deficit of 10-14 points maintains an 18 percent comeback rate, and the 20+ comeback attempts are below 3 percent. Within the 3-point variance, teams that shoot significantly more three-point attempts and have higher 3pt percentage differentials have a much higher chance of a comeback compared to teams that get blown out. Finally, for the turnover creation, a negative turnover differential is correlated with more positive scoring runs.

5.

Games with incomplete play-by-play logs accounted for less than 0.5 percent of the samples which were dropped. Outliers within the pace and box score statistics were truncated at the 99th percentile. is_home is converted to binary 1/0 and the numerical features such as score_margin, seconds_remaining, and trailing_3pt_pct etc are standardized and we are using StandardScaler for logistic regression. Tree Boost XGBoost models used raw features that were used to maintain split threshold readability. The features were also selected through 4 different domains such as game state which includes score_margin, seconds_remaining,, period and is_home, shooting drivers which includes trailing_3PAR and traiing_3p_pct and posession and momentum which includes turnover_diff,orb_pct_diff, and rolling_pace and finally for the prior context we need the pregame_spread. 

The data was spread more through chronologically first through (2020-2023 for training and 2023-2024 for testing) and yeah instead of more randomly replicating real world coaching deployment. Data leakage was prevented by ensuring that the efficiency features reflect the statistics were accumulated up to the specific timestamp such as halftime and it would also exclude all downstream 2nd half/4th quarter box score stats.









