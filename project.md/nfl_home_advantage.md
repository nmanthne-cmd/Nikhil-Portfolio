Problem Definition: 

I wanted to see whether playing at home in the NFL provides a measurable advantage over the years and across the playoffs as a whole. Based on my research, it has shown me that home-court advantage has measurably declined over the years. Historically, playing at home granted NFL teams a 3.0 scoring cushion that guaranteed a 57 - 58 winning percentage and this was more attributed to referee bias, visitor fatigue, venue familiarity, and more. However, when looking at game tracking from ESPN and nflverse GitHub, it has shown that this has dropped significantly and the baseline home winning percentage is now sitting between 53.7 and 54.8, placing HFA between 1.0 and 1.5 points.

Data Description: 

As I mentioned before, I want to measure whether home-court advantage has declined over the years. Within the 2000 - 2008 era in my research I have noticed that home teams have maintained a 57 percent win rate with an average margin of 2.5 - 2.7 points. But from 2015 - 2026, the home winning rate has reduced to 54 percent, with a scoring margin of near 1.2 points. Also, accounting for the 2020 Covid 19 era, the home-venue winning rate dropped to 49.8 that was a drop from historical norms, and the home crowd audience is a major contributor to home-winning advantage. But more specifically, in the playoff games, the home winning percentage jumps up to 62-69 percent, according to the NFLverse GitHub. More specifically in more neutral sites though there is a more of 50/50 baseline as a whole but within rest days as a whole is more of a 3 to 5 percent boost as a whole and more specifically this is for Thursday games that are at home favor more of the visting teams for three days of rest.

Also, within this analytical study, we are going through 26 complete NFL seasons from 2000-2026 and, more specifically, the 6832 total non-neutral sites at the schedule level that are paired with over 1.25 million play-by-play events. Neutral sites such as the ones in London, Germany, Brazil, and Super Bowl contests are excluded to provide fairness. Now, within the playoff setting from 2000-2018, the data I have seen shows a 64.2 win rate, whilst from 2019- 2025 there is a 62.5 win rate percentage with a small drop. When also analyzing short rest that visitors come into, there is a 60.1 percent win rate from 2000-2018 and 58.3 percent from 2019- 2025, with a small drop. Also, accounting for COVID, it was a 50.4 percent win rate as a whole.

Data Cleaning and Preparation: 

For data cleaning purposes, I chose a metric that would actually measure home-field advantage. I more specifically decided on a simple scoring margin and win probability concept that counted the game outcomes and seeing if home teams were outperforming visiting opponents. Since I was working within multiple datasets play by play from nflverse github and using ESPN QBR metrics.

<img width="662" height="87" alt="image" src="https://github.com/user-attachments/assets/f0259863-055e-430b-a430-a22aa4c98980" />

After further exploratory data analysis that took me 2-3 hours, I was able to notice that the unique game counts and team identifiers did contribute to edge cases such as the international series matchups, relocated franchise abbreviations(SD vs LAC, OAK vs LV), and Super Bowl games that were played at neutral venues. I also worked on using consistency spanning the 2000-2025 seasons as a whole.

<img width="846" height="222" alt="image" src="https://github.com/user-attachments/assets/c8148b0e-a5a7-45e2-819a-62adb10bac81" />

This method would take out the 


