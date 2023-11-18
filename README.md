# League Meta Health

by Lukas Fullner, Justin Lu

---

## Introduction

---

## Cleaning and EDA

<iframe src="assets/10-80-enrollment.html" width=800 height=600 frameBorder=0></iframe>

Our dataset given was a collection of tournament results across the year 2023, containing match information from many major
tournaments throughout the year.  The dataset has many columns about various game stats, but we cut it down to:

- `"gameid"`: A unique id for every game played.  this is useful because each game has 10 players, but there are actually
  12 rows per game, so knowing the game id helps us parse the actual unique characters.
- `"patch"`: This is a float representing what version of the game the competition is played on, ranging from 13.01 to 13.21
  with some missing patches.  We compute meta health per patch then average the changes between them as our statistic.
- `"champion"`: This represents what champion a player picked.  Notably rows 11 and 12 per game are null as these just store
  bans
- `"ban1"`: This is the first champion banned by each team, and is the same for rows 1-5 and 6-10 per game.
- `"ban2"`: Same as `"ban1"` but for the second ban.
- `"ban3"`: Same as `"ban1"` but for the third ban.
- `"ban4"`: Same as `"ban1"` but for the fourth ban.
- `"ban5"`: Same as `"ban1"` but for the fifth ban.

Notably we don't store the result of the games, as we don't care which characters are winning and losing, just that characters are being represented.

here is one game from the dataset, note how the champions are arranged.

'| gameid                |   patch | champion   | ban1   | ban2    | ban3   | ban4   | ban5   |\n|:----------------------|--------:|:-----------|:-------|:--------|:-------|:-------|:-------|\n| ESPORTSTMNT06_2753012 |   13.01 | Jax        | Sylas  | Caitlyn | Wukong | Akali  | Yone   |\n| ESPORTSTMNT06_2753012 |   13.01 | Poppy      | Sylas  | Caitlyn | Wukong | Akali  | Yone   |\n| ESPORTSTMNT06_2753012 |   13.01 | Taliyah    | Sylas  | Caitlyn | Wukong | Akali  | Yone   |\n| ESPORTSTMNT06_2753012 |   13.01 | Ezreal     | Sylas  | Caitlyn | Wukong | Akali  | Yone   |\n| ESPORTSTMNT06_2753012 |   13.01 | Karma      | Sylas  | Caitlyn | Wukong | Akali  | Yone   |\n| ESPORTSTMNT06_2753012 |   13.01 | Sejuani    | Galio  | Lucian  | Fiora  | Viktor | Azir   |\n| ESPORTSTMNT06_2753012 |   13.01 | Viego      | Galio  | Lucian  | Fiora  | Viktor | Azir   |\n| ESPORTSTMNT06_2753012 |   13.01 | Syndra     | Galio  | Lucian  | Fiora  | Viktor | Azir   |\n| ESPORTSTMNT06_2753012 |   13.01 | Zeri       | Galio  | Lucian  | Fiora  | Viktor | Azir   |\n| ESPORTSTMNT06_2753012 |   13.01 | Yuumi      | Galio  | Lucian  | Fiora  | Viktor | Azir   |\n| ESPORTSTMNT06_2753012 |   13.01 | nan        | Sylas  | Caitlyn | Wukong | Akali  | Yone   |\n| ESPORTSTMNT06_2753012 |   13.01 | nan        | Galio  | Lucian  | Fiora  | Viktor | Azir   |'

---

## Assessment of Missingness

Here's what a Markdown table looks like. Note that the code for this table was generated _automatically_ from a DataFrame, using

```py
print(counts[['Quarter', 'Count']].head().to_markdown(index=False))
```

| Quarter     |   Count |
|:------------|--------:|
| Fall 2020   |       3 |
| Winter 2021 |       2 |
| Spring 2021 |       6 |
| Summer 2021 |       4 |
| Fall 2021   |      55 |

---

## Hypothesis Testing

Hello world!

---
