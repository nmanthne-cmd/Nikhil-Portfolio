The problem statement is  to identify whether an NBA team trailing by double digits at a live game checkpoint can overcome the deficit and win the game. For the target variable, I am looking to assess comeback_win, which would be 1 if the trailing team wins or 0 if the trailing team loses. For this task type, it would be assessed as binary classification, and the people who would primarily benefit from this would be sports analytics staff and head coaches, who would figure out whether to bench starters vs. actually trying to push for a comeback; the sports media, who are gonna work on using this content as framing more narrative shifts as a whole and it would also be used in live betting models. The significance of this would be that standard win probability relies heavily on static score differential and more of the time remaining. This high-variance basketball as a whole should make the deficits easier to come back from through 3-point shooting. This can also help us understand the best live game mechanics that would trigger a comeback by isolating the more team-driven performance from the other random scoring noise as a whole.

Modern NBA Analytics have established that lead security has dropped a lot over the past decade. The adoption of three-point shooting and increased pace of play has introduced more variance within the live game outcomes. Within a traditional win probability framework, evaluate the win probability as a function of point differential, time remaining, and home-court advantage. (Lock and Schuckers 2013) However, the teams that are trailing can alter the expected points per possession by shifting the more tactical levers, more specifically by increasing their three-point attempt rates and pressing for more turnovers. Taking into account research suggests that trailing teams benefit from the regression to the mean within shooting efficiency, and primarily through other plays, like where leading teams offensively 'stall,' disrupting trailing teams' rhythm. (Oliver 2004, Skinner 2012)

Sources: 
Lock, K., & Schuckers, M. (2013). In-game web-based win probability models for the National Basketball Association. Journal of Quantitative Analysis in Sports, 9(2), 197–205.

Oliver, D. (2004). Basketball on Paper: Rules and Tools for Performance Analysis. Brassey's.

Skinner, B. (2012). The problem of shot selection and the price of anarchy in basketball. Journal of Quantitative Analysis in Sports, 8(1), 1–16.

Data Sources and Links: 

https://github.com/swar/nba_api


