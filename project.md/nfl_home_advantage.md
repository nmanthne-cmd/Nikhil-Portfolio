Problem Definition: 

I wanted to see whether playing at home in the NFL provides a measurable advantage over the years and across the playoffs as a whole. Based on my research, it has shown me that home-court advantage has measurably declined over the years. Historically, playing at home granted NFL teams a 3.0 scoring cushion that guaranteed a 57 - 58 winning percentage, and this was more attributed to referee bias, visitor fatigue, venue familiarity, and more. However, when looking at game tracking from ESPN and nflverse GitHub, it has shown that this has dropped significantly, and the baseline home winning percentage is now sitting between 53.7 and 54.8, placing HFA between 1.0 and 1.5 points.

Data Description: 

As I mentioned before, I want to measure whether home-court advantage has declined over the years. Within the 2000 - 2008 era in my research, I have noticed that home teams have maintained a 57 percent win rate with an average margin of 2.5 - 2.7 points. But from 2015 - 2026, the home winning rate has reduced to 54 percent, with a scoring margin of near 1.2 points. Also, accounting for the 2020 Covid 19 era, the home-venue winning rate dropped to 49.8 that was a drop from historical norms, and the home crowd audience is a major contributor to home-winning advantage. But more specifically, in the playoff games, the home winning percentage jumps up to 62-69 percent, according to the NFLverse GitHub. More specifically, in more neutral sites, though, there is more of a 50/50 baseline as a whole, but within rest days as a whole is more of a 3 to 5 percent boost as a whole, and more specifically, this is for Thursday games that are at home, favoring more of the visiting teams for three days of rest.

Also, within this analytical study, we are going through 26 complete NFL seasons from 2000-2026 and, more specifically, the 6832 total non-neutral sites at the schedule level that are paired with over 1.25 million play-by-play events. Neutral sites such as the ones in London, Germany, Brazil, and Super Bowl contests are excluded to provide fairness. Now, within the playoff setting from 2000-2018, the data I have seen shows a 64.2 win rate, whilst from 2019- 2025 there is a 62.5 win rate percentage with a small drop. When also analyzing short rest that visitors come into, there is a 60.1 percent win rate from 2000-2018 and 58.3 percent from 2019- 2025, with a small drop. Also, accounting for COVID, it was a 50.4 percent win rate as a whole.

Data Cleaning and Preparation: 

For data cleaning purposes, I chose a metric that would actually measure home-field advantage. I more specifically decided on a simple scoring margin and win probability concept that counted the game outcomes and seeing if home teams were outperforming visiting opponents. I was working within multiple datasets, including play-by-play from the NFLverse GitHub and using ESPN QBR metrics.

<img width="662" height="87" alt="image" src="https://github.com/user-attachments/assets/f0259863-055e-430b-a430-a22aa4c98980" />

After further exploratory data analysis that took me 2-3 hours, I was able to notice that the unique game counts and team identifiers did contribute to edge cases such as the international series matchups, relocated franchise abbreviations(SD vs LAC, OAK vs LV), and Super Bowl games that were played at neutral venues. I also worked on using consistency spanning the 2000-2025 seasons as a whole.

<img width="846" height="222" alt="image" src="https://github.com/user-attachments/assets/c8148b0e-a5a7-45e2-819a-62adb10bac81" />

This method would take out the neutral-site venues such as Wembley Stadium or the Super Bowl, and because there is no home-field advantage in these areas, they do not count.

Through my next process, which took me 1-2 days, I decided to filter the games by historical differences and rest differentials. I chose to evaluate through these three eras: 2000- 2008, 2009 - 2018, and 2019 - 2025. Through this, I am also adding a sports science concept called travel friction. This would more specifically evaluate when visitors travel through multiple time zones and play on short rest, and it also measures whether travel exhaustion impacts team performance.

<img width="905" height="92" alt="image" src="https://github.com/user-attachments/assets/2567e5d1-73f0-4f73-914c-7dfcfea98b17" />

I then worked on filtering play by play that would evaluate penalty events that took me an additional 1-2 days and accounting for events such as false starts, delay of game penalties, and more specifically, burned timeouts from the visiting offence which also accounted for errors from blowouts that would not take into account for the crowd noise.

<img width="900" height="46" alt="image" src="https://github.com/user-attachments/assets/74436071-d7ec-4a49-9840-91562571c632" />

Finally, something that took me around 4-5 days was accounting for getting my counts that would measure environmental friction due to visitor disruptions. I then worked on making separate counts for regular season games vs. playoff games and knowing that playoff games feature higher crowd rowdiness and stakes. Once I accounted for all of these, I merged them all into a dataframe for visualization and analysis.

<img width="832" height="197" alt="image" src="https://github.com/user-attachments/assets/d50a82f5-2e92-40c0-a456-8f7722390db6" />
<img width="906" height="162" alt="image" src="https://github.com/user-attachments/assets/b9a8cbed-9389-41ef-8163-c748408afdbd" />

Data Understanding and Visualizations:

In order to get a better feel for this dataframe, I generated a distribution of home winning percentages, point diffrentials, and visitor presnap penalty counts through different historical counts.

While the summaries don't tell us the full story, the distribution of point differentials shows us that there is a difference from zero over time. Between 2000-2008, the home potential distribution was symmetric but centered around 2.7 points but within the modern era spanning from 2019- 2025, it has stabilized to 1.2 points. I also ended up doing this same exploratory analysis which also took me around 2-3 days with stadium crowd capacities and attendance percentages but there was one anomaly, which was the 2020 season.

<img width="727" height="242" alt="image" src="https://github.com/user-attachments/assets/4343d257-7900-4b63-853e-754bf7ab6c6c" />

During 2020, though, when stadium participation dropped to zero due to restrictions, the home win percentage dropped to 50.4 percentage. Within the context of this, live crowd noise, more so than the stadium or the turf, contributed the most to the home field advantage. I also followed this up with a regression plot that measures pre-snap penalties against final home margins to see whether the crowd affects the larger victories.

<img width="807" height="470" alt="image" src="https://github.com/user-attachments/assets/be2de629-849a-4208-85aa-78f41034441d" />
<img width="897" height="476" alt="image" src="https://github.com/user-attachments/assets/cbf9ac21-6f67-4a58-b605-691e1c81cac1" />

Also, to work on evaluating whether or not crowd disruption would affect larger victories, I followed it up with a least-squares regression line and scatter plot as a whole that would compare the victor's pre-snap penalties against final home margins, which took me around a week to do. While the fitted line would show a more positive slope that would account for more visiting false starts and delay-of-game penalties that would correspond to larger home margins, the variance would remain very high, equating to r2 = 0.04. Most of the games do cluster around 0 to 3 visitor pre-snap penalties, and they do produce point differentials that range from -20 to 20, and this more so shows that crowd noise creates more local noise for the visiting offenses, and also the bigger variables such as quarterback efficiency and turnover margin that would show more than just the scoreboard affects it.

















