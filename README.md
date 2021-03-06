[![Code Climate](https://codeclimate.com/github/bwaggone/smash-stats/badges/gpa.svg)](https://codeclimate.com/github/bwaggone/smash-stats) [![Issue Count](https://codeclimate.com/github/bwaggone/smash-stats/badges/issue_count.svg)](https://codeclimate.com/github/bwaggone/smash-stats)
#Smash.gg Data Scraper and Statistics Aggregator
##A Project Intended to Aggregate Statistics for Smash Bros. Players.
###Version: 0.1.0

Below is a general list of tasks to get done, stretch goals, and what has actually been accomplished. I don't think this is really ready for an official release yet, but it is *usable*. Barely.

####DONE
* Smash.gg API Scraper
* Data file structure
* Have master tournament files with a list of slugs + dates + entrants
* 64/Melee/Brawl/PM/Smash4 Support (Scraping)
* Multithreaded support to scrape more than one tournament at a time

####TO-DO
* Write an update function that will grab all tournaments yet to be recorded, update the data, fix any possible name discrepencies, and move (or link) the data to a folder of the user's choosing. (For use with /smash-site-static/)
* Add a mode for top 8/32/64 ONLY. Might show a different side of the data.
* Glicko Calculator
  * Add support for more than one Calendar year + tournament timeout + Arbitrary start date
  * Add a weighting mechanic, because this shit looks really funky
* Add exceptions to calculator, ignore results from player X on tournament Y.
* Scrape from challonge

####KNOWN ISSUES
* People with pipes (|) in their tag. *Currently no way to distinguish a pipe and a sponsor separator without bombarding smash.gg with a billion API requests*. This is not an issue for doubles files, only singles.
* Does **not** work on tournaments that run games other than smash. (Rivals of Aether, other fighting games).
* **EVO is excluded because it is not hosted on smash.gg. It will have to be added manually to the data, or another method needs to be used. **

####Future Goals:
* Make the glicko calculator standalone (maybe a python package?)
* Add a predictor for a head to head given current ratings (using different rating systems)
* Use the predictor to model the likelihood of player X winning tournament Y

### Data Guarantees and Glicko
Most of the information I've gathered about glicko has come from both wikipedia, and a pdf [from glicko himself.](http://www.glicko.net/glicko/glicko.pdf) The glicko system can be **VERY** volatile. Players who don't compete very often but have a high skill tend to have their rankings change more drastically than their peers who compete more often. Therefore, a player's rating is more likely to change if they're new, or haven't competed in awhile. This does lead to some anomolies with rankings and scores, but remember, glicko is intended to be interpreted as a **range** of possible skill values, not a true skill value.

For example, if we have a player who's rating is 1800 and they have a rating deviation of 67, we are 95% confident that their true rating is between 1800 +/- (2\*67).

Based on how the data is gathered, it's easy for data to be missed or attributed incorrectly. Cases where a player is known by more than one name, typo'd in tournament, or two players have the same name can skew this data (albiet not by much unless you're a certain smash4 player who thinks it's cool to change your tag every other month). In an ideal world, if everyone had roughly the same amount of matches, these rankings would be more representative, but hey, that's what statistics is supposed to tell you anyway. Take the ratings with a grain of salt, and I'll eventually try to find ways to lower the variance or provide multiple views on the data.

### Trueskill
[Trueskill](http://trueskill.org/) is a method of ranking/rating that Xbox live uses for multiplayer games. It requires less activity to get a more accurate picture of ranking, but follows a similar interpretation to Glicko. Trueskill provides a mean (mu) and standard deviation(sigma), so a player's skill is said to follow a normal distribution, and we can say we're 95% confident that their true skill is between mu +/- (2\*sd). It's a different scale, and for more information on how Trueskill is calculated, [see the appropriate papers written by microsoft](https://www.microsoft.com/en-us/research/publication/trueskilltm-a-bayesian-skill-rating-system/). For a one sentence overview of Trueskill: It uses Bayesian Inference.
