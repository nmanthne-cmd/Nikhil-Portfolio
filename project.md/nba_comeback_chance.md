The problem statement is  to identify whether an NBA team trailing by double digits at a live game checkpoint can overcome the deficit and win the game. For the target variable, I am looking to assess comeback_win, which would be 1 if the trailing team wins or 0 if the trailing team loses. For this task type, it would be assessed as binary classification, and the people who would primarily benefit from this would be sports analytics staff and head coaches, who would figure out whether to bench starters vs. actually trying to push for a comeback; the sports media, who are gonna work on using this content as framing more narrative shifts as a whole and it would also be used in live betting models. The significance of this would be that standard win probability relies heavily on static score differential and more of the time remaining. This high-variance basketball as a whole should make the deficits easier to come back from through 3-point shooting. This can also help us understand the best live game mechanics that would trigger a comeback by isolating the more team-driven performance from the other random scoring noise as a whole.

Modern NBA Analytics have established that lead security has dropped a lot over the past decade. The adoption of three-point shooting and increased pace of play has introduced more variance within the live game outcomes. Within a traditional win probability framework, evaluate the win probability as a function of point differential, time remaining, and home-court advantage. (Lock and Schuckers 2013) However, the teams that are trailing can alter the expected points per possession by shifting the more tactical levers, more specifically by increasing their three-point attempt rates and pressing for more turnovers. Taking into account research suggests that trailing teams benefit from the regression to the mean in shooting efficiency, and primarily through other plays, like where leading teams offensively 'stall,' disrupting trailing teams' rhythm. (Oliver, 2004; Skinner, 2012)

Sources: 

Lock, K., & Schuckers, M. (2013). In-game web-based win probability models for the National Basketball Association. Journal of Quantitative Analysis in Sports, 9(2), 197–205. https://doi.org/10.1515/jqas-2012-0051

Oliver, D. (2004). Basketball on Paper: Rules and Tools for Performance Analysis. Brassey's.

Skinner, B. (2012). The problem of shot selection and the price of anarchy in basketball. Journal of Quantitative Analysis in Sports, 8(1), 1–16. https://doi.org/10.1515/1559-0410.1344

Stern, H. S. (1994). A preliminary statistical analysis of the home field advantage in professional sports. Journal of Quantitative Analysis in Sports, 1(1), 1–15.


Data Sources and Links: 

https://github.com/swar/nba_api
https://www.basketball-reference.com/leagues/NBA_2024_games.html

Within the datastructure and limitations, a single NBA game snapshot is recorded at a specific game checkpoint: halftime or the end of the 3rd quarter, where the leading team forces a deficit of 10 or more points. We are going to be looking at 4 datasets from 2020-2021 all the way through 2023-2024, which would cover 4920 regular season games. I would primarily be filtering for instances where a team trailed by 10 or more points or if the 3rd-quarter break, which yielded a smaller dataset of 1428 unique comeback opportunity snapshots. My target variable would be comeback_win and it would be binary: 0 if the trailing team loses and 1 if the trailing team wins. THe primary available features that I am primarily looking at are score_margin




