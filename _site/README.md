<!-- # Meta Health Analysis per Patch in Competitive League of Legends -->

by Lukas Fullner (lfullner@ucsd.edu) and Justin Lu (jzlu@ucsd.edu)

## Introduction

This study looks into the idea of a "meta" and its health in the competitive video game scene of League of Legends. More specifically, are the patch updates that Riot Games (the game developer) publishes making the meta healthier?

Just like how different athletes have different strengths in physical sports, "champions" with unique capabilities have different relative strengths to one another in esports. In League of Legends, each player chooses one champion to play, which is often one of the most influential choices they can make. In professional play, teams of 5 players each must take turns choosing their champions, where only one team can pick any given champion out of the 165 total available. In this process called a "draft", each team can also ban 5 champions, meaning both teams cannot pick them. Teams take turns picking and banning, ultimately resulting in 10 unique champions chosen and 10 unique champions banned. 

Naturally, both professional and casual players will try to find the best champion at any given time in order to give them the greatest chance to win. This leads to a "meta" (or "most efficient tactic available"), which is a term referring to the best options available to a player. Our goal with this project was to answer the question of how healthy the meta is, but how does one even define meta health?  

Since there are so many champions available, but so few slots for presence in a game, we chose to define our statistic as where: a healthy meta is one where the largest quantity of characters are seen as strong enough for professinal play. We can determine this figure by using "meta presence", which represents how often a character appears as either picked and banned. More specifically we computed this term by counting every occurance of a character in a given period, then dividing that by the total number of characters picked and banned in that time period. Then, we looked at how much of the total presence did the top 50 champions comprise, where lower numbers are healthier.

So what time period are we using? Every year, Riot Games (the game developer) will monitor the relative strength of each character, and tune their abilities to hopefully be more in line with the others, aka keeping the meta healthy. To both accomplish their goal of consistent maintenance, but also not change things so fast that players can't keep up, they "patch" the game about once every 2-3 weeks. During this time, players adapt and learn the new changes, and try to reestablish what the strongest champion picks are.

To conclude, we will be examining how a meta (the set of champions that is present in professional play) evolve in a year, and whether they become "healthier" (more champion diversity) throughout that time, broken up by each "patch".

---

## Cleaning and EDA

Our dataset given was a collection of tournament results across the year 2023, containing match information from many major tournaments throughout the year.  The dataset has many columns about various game stats, but we cut it down to:

- `"gameid"`: A unique id for every game played. This is useful because each game has 10 players, but there are actually 12 rows per game (one for each team's overall stats). Thus, knowing the `gameid` helps us parse the actual unique characters.
- `"patch"`: This is a float representing what version of the game the competition is played on, ranging from 13.01 to 13.21 with some missing patches. We compute meta health per `patch` then average the changes between them as our statistic.
- `"champion"`: This represents what `champion` a player picked. Notably, the 11th and 12th row for every game are null as these just store overall team statistics and are not designed to store champion picks
- `"ban1"`: This is the first champion banned by each team, and is the same for rows 1-5 and 6-10 per game.
- `"ban2"`: Same as `ban1` but for the second ban.
- `"ban3"`: Same as `ban1` but for the third ban.
- `"ban4"`: Same as `ban1` but for the fourth ban.
- `"ban5"`: Same as `ban1` but for the fifth ban.

Notably we don't store the result of the games, as we don't care which characters are winning and losing, just that characters are being represented. This is since being either chosen or ban reflects the professional teams' belief that a given character is strong.

Here is one game from the dataset; note how the champions are arranged with the 11th and 12th row show the overall team's bans in order, and do not have individual champion picks since they reflect the entire team.

| gameid                |   patch | champion   | ban1   | ban2    | ban3   | ban4   | ban5   |
|:----------------------|--------:|:-----------|:-------|:--------|:-------|:-------|:-------|
| ESPORTSTMNT06_2753012 |   13.01 | Jax        | Sylas  | Caitlyn | Wukong | Akali  | Yone   |
| ESPORTSTMNT06_2753012 |   13.01 | Poppy      | Sylas  | Caitlyn | Wukong | Akali  | Yone   |
| ESPORTSTMNT06_2753012 |   13.01 | Taliyah    | Sylas  | Caitlyn | Wukong | Akali  | Yone   |
| ESPORTSTMNT06_2753012 |   13.01 | Ezreal     | Sylas  | Caitlyn | Wukong | Akali  | Yone   |
| ESPORTSTMNT06_2753012 |   13.01 | Karma      | Sylas  | Caitlyn | Wukong | Akali  | Yone   |
| ESPORTSTMNT06_2753012 |   13.01 | Sejuani    | Galio  | Lucian  | Fiora  | Viktor | Azir   |
| ESPORTSTMNT06_2753012 |   13.01 | Viego      | Galio  | Lucian  | Fiora  | Viktor | Azir   |
| ESPORTSTMNT06_2753012 |   13.01 | Syndra     | Galio  | Lucian  | Fiora  | Viktor | Azir   |
| ESPORTSTMNT06_2753012 |   13.01 | Zeri       | Galio  | Lucian  | Fiora  | Viktor | Azir   |
| ESPORTSTMNT06_2753012 |   13.01 | Yuumi      | Galio  | Lucian  | Fiora  | Viktor | Azir   |
| ESPORTSTMNT06_2753012 |   13.01 | nan        | Sylas  | Caitlyn | Wukong | Akali  | Yone   |
| ESPORTSTMNT06_2753012 |   13.01 | nan        | Galio  | Lucian  | Fiora  | Viktor | Azir   |

In computing our test statistic later we will count the champions in rows 1-10, then all the bans from rows 11 and 12.

### Univariate Analysis:

<iframe src="assets/univariate.html" width=800 height=600 frameBorder=0></iframe>

This graph shows the count of every champion in the dataset. While the actual values aren't too important, it is relevant to see that some champions see a lot more play than others in the 2023 season.

### Bivariate Analysis:

<iframe src="assets/bivariate.html" width=800 height=600 frameBorder=0></iframe>

This graph shows the number of games played per patch. This is useful in showing that the total number of games played thoughout the lifecycle of a season fluctuates, and could have an impact on our results as different patches have a different sample size.

### Interesting Aggregate:

| champion     |   13.01 |   13.03 |   13.04 |   13.05 |   13.06 |   13.07 |   13.08 |   13.09 |   13.1 |   13.11 |   13.12 |   13.13 |   13.14 |   13.15 |   13.16 |   13.17 |   13.18 |   13.19 |   13.2 |   13.21 |
|:-------------|--------:|--------:|--------:|--------:|--------:|--------:|--------:|--------:|-------:|--------:|--------:|--------:|--------:|--------:|--------:|--------:|--------:|--------:|-------:|--------:|
| Aatrox       |      65 |      17 |      19 |      18 |       1 |       3 |       8 |       4 |     10 |      19 |       6 |      17 |     221 |      30 |       3 |       8 |      11 |      63 |     11 |       0 |
| Ahri         |      85 |      40 |     124 |     214 |      88 |      56 |     120 |      27 |    188 |     366 |     224 |     193 |     169 |      51 |       8 |      45 |      22 |      46 |      3 |       1 |
| Akali        |     333 |     131 |     107 |      75 |       8 |       5 |      11 |       4 |      9 |      44 |      44 |      44 |      17 |       5 |       0 |       8 |       7 |      38 |     11 |       0 |
| Akshan       |       4 |       0 |       4 |       2 |       2 |       0 |       0 |       0 |      1 |       4 |       1 |       8 |       3 |       1 |       0 |       3 |       1 |       3 |      0 |       0 |
| Alistar      |      43 |      37 |      32 |      25 |       7 |      18 |      18 |       6 |     16 |      22 |     209 |     258 |     230 |     109 |      12 |      73 |      57 |      96 |     10 |       3 |
| Amumu        |      82 |      41 |      23 |      14 |       4 |       1 |       1 |       0 |      9 |      14 |       7 |      38 |      34 |       5 |       0 |       5 |       4 |       3 |      1 |       0 |
| Anivia       |       2 |       3 |       6 |       1 |       0 |       0 |       0 |       0 |      0 |       0 |       0 |       2 |       0 |       0 |       0 |       0 |       1 |       0 |      0 |       0 |
| Annie        |       1 |     215 |     258 |     182 |      40 |      29 |      56 |      17 |    145 |     316 |     219 |      94 |      32 |       4 |       2 |       0 |       0 |       0 |      0 |       0 |
| Aphelios     |      67 |      63 |     322 |     307 |      45 |      70 |     148 |      39 |    297 |     696 |     364 |     223 |     132 |      26 |       5 |      10 |      12 |      69 |      4 |       2 |
| Ashe         |     423 |     143 |     110 |      42 |       4 |       3 |       2 |       1 |     15 |      14 |     130 |     178 |      62 |       1 |       2 |      15 |      12 |      31 |      7 |       0 |
| Aurelion Sol |       0 |       0 |      76 |      50 |       5 |       1 |       6 |       0 |     12 |       8 |       7 |       5 |       3 |       0 |       0 |       1 |       0 |       2 |      0 |       0 |
| Azir         |     518 |     186 |     147 |       7 |       5 |       4 |       1 |       2 |      8 |     201 |     286 |     327 |     273 |      74 |      10 |      33 |      15 |      96 |      9 |       3 |
| Bard         |       9 |       1 |       0 |       2 |       0 |       0 |       0 |       0 |      0 |       1 |       0 |       3 |       2 |       0 |       2 |       0 |       4 |       7 |      2 |       0 |
| Bel'Veth     |      15 |       8 |      16 |       8 |       5 |       0 |       1 |       0 |      8 |       5 |      22 |      13 |      15 |       1 |       1 |       4 |      10 |      15 |      0 |       0 |
| Blitzcrank   |      29 |       5 |      44 |      74 |      25 |      15 |      25 |       9 |     24 |      76 |      80 |      54 |      29 |       5 |       0 |       5 |       5 |      18 |      1 |       0 |
| Brand        |       3 |       0 |       1 |       0 |       0 |       0 |       0 |       0 |      1 |       0 |       0 |       0 |       0 |       0 |       0 |       0 |       0 |       0 |      0 |       0 |
| Braum        |      82 |      56 |      46 |      50 |      30 |      11 |      22 |       5 |     39 |      63 |     117 |     195 |     124 |      29 |       4 |      22 |      16 |      26 |      6 |       1 |
| Briar        |       0 |       0 |       0 |       0 |       0 |       0 |       0 |       0 |      0 |       0 |       0 |       0 |       0 |       0 |       0 |       0 |       0 |       3 |      3 |       1 |
| Caitlyn      |     304 |     144 |      93 |      46 |       6 |       5 |       0 |       0 |      1 |       1 |       0 |       1 |       1 |       0 |       7 |       4 |       2 |      12 |      3 |       0 |
| Camille      |     165 |      56 |      57 |      28 |       6 |       7 |       5 |       6 |     16 |      18 |       7 |      14 |       8 |       7 |       4 |       8 |       7 |       8 |      1 |       0 |

This is the first 20 entries of champion picks per patch. It shows two interesting aspects of patches: first that some champions will see more picking throughout the season compared to others, and second that others will fluctuate heavily throughout the season.

---

## Assessment of Missingness

### NMAR
When looking at the data, we found that much of the missingness was tied to the `league` that recorded it. Some `league`'s didn't record stats, some didn't subclassify, and some formatted them differently. This would lead us to feel that there were mostly columns that were missing by design (MD), rather than not missing at random (NMAR). This is since given the name of a league, you can absolutely determine if data will be missing, since it was not recorded (by design) and not related to the value of the missing data itself.

### MAR: `split` on `league`
Since different `league`s will change how their tournament formats are run, they can change the names and amount of `split`s that they have over time. 

For example, the `league` called `LFL2` changed their labelling throughout the season:

---

| gameid                | league   | split   | date                |   patch |
|:----------------------|:---------|:--------|:--------------------|--------:|
| ESPORTSTMNT06_2753012 | LFL2     | Spring  | 2023-01-10 17:07:16 |   13.01 |
| ESPORTSTMNT06_2753012 | LFL2     | Spring  | 2023-01-10 17:07:16 |   13.01 |
| ESPORTSTMNT04_2697714 | LFL2     | nan     | 2023-09-15 19:57:10 |   13.17 |
| ESPORTSTMNT04_2697714 | LFL2     | nan     | 2023-09-15 19:57:10 |   13.17 |

---

Thus, we were interested to see if `split` would be MAR on `league`, and ran a permutation test using TVD as our test statistic. We'll use the standard significance level of 0.05.

<iframe src="assets/split_on_league.html" width=800 height=600 frameBorder=0></iframe>

Our observed p-value we saw was 0.0, which is much below 0.05, indicating to us that a correlation (MAR) between the missingness of `split` and `league`.

### MCAR: `ban1` on `result`
While we noticed some bans were missing, we found that they were not significantly conditional on the results of a game, win or loss. To measure this, we ran a permutation test to see if there was a correlation between the the missingness of `ban1` and `result`, and used TVD for our test statistic. For this, we'll still use the standard significance level of 0.05.

<iframe src="assets/ban1_on_result.html" width=800 height=600 frameBorder=0></iframe>

The observed p-value we saw for our TVD was 0.59, which would be far too high for our significance level of 0.05. This indicates to us that there is no correlation (MCAR) between the missingness of `ban1` and `result`.

---

## Hypothesis Testing

**Null Hypothesis:** The average change in meta health throughout a season zero.
**Alternate Hypothesis:** The average change in meta health is negative. This means the meta is getting healthier throughout the season, since the top 50 champions dominate less.
**Test Statistic:** Our test statistic is the average change in meta health, calculated by finding what proportion of the meta the top 50 champions make up per patch, then averaging the difference between them.
**Significance level:**  We chose a standard 0.05 or 5% significance
**P-value:** 0.369
- This was computed with 1000 simulations

<iframe src="assets/p-val.html" width=800 height=600 frameBorder=0></iframe>

**Conclusion:** We see here that our results were not significant, meaning we fail to reject the null and cannot say if Riot Games' Balance team is making the meta healthier throughout the season. We would like to mention the observed average health change is slightly positive, implying metas actually get slighty more unhealthy as the season progresses. As stated with our analysis this is not statistically relavent and is not proof of anything. However, this could be representative of how players make the meta less healthy as they converge on what the optimal strategies are.