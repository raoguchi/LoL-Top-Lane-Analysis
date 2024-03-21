by Ryosuke (Alex) Oguchi

## Introduction

The dataset we are examining here is the 2022 data for League of Legends Esports Matches. The dataset is subset in groups of 12, which the corresponding match ID. However, as there are only 10 players in each match (5 per team), there are 10 rows about player data and 2 rows about team data. When I claim a champ is "broken", there are various metrics we can use to test this claim. Since this is examining perfromance based data on champions/players, any objective based data will not be part of our model. Moreover, objective based data is a team performance statistic that is "Missing by Design" for individual statistics. Nevertheless, we can assume good top laner performance will contribute to the overall objective completion for each team. Although this may be contingent on the other player's performance (especially the jungler), lane pressure will create a situation where the wave is shoved in due to power difference between the two players. This allows the jungler to take claim to the Rift Herald which is directly related to taking towers.

Just as a discretion, Malphite's build path tends to take one of two paths: Ability Power and Tank. As the latter suggests, the Tank build focuses on building up Damage Resistance by sacrificing damage output. The key reason why this question is being asked is simply due to this Ultimate Ability.

As described in the official League of Legends Website, it is described as follows: "UNSTOPPABLE FORCE: Malphite launches himself to a location at high speed, damaging enemies and knocking them into the air."

Because this move is quite literally unstoppable, it has the power to influence individual match-ups and team fights into one-sided affairs.

<iframe width="560" height="315" 
src="https://www.youtube.com/embed/dQGwo_MA_3c?si=OwPtOWG0xlDqKKEo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen>
</iframe>


Now that I have established Malphite is not fair, it is necessary to narrow down what type of Data is relevant for analysis. What we are trying to answer is what is the predicted amount of total objectives (dragons, heralds, barons, towards, inhibitors, etc.) that a team will get based off the existence of an "effective" Malphite?

As for individual statistics, we will examine, the damage, damage taken, the vision score, the cs, the kills, the deaths, the assists, and objectives.

- Damage to Champions: How much damage a champion has dealt to other champions in total
- Damage Taken: How much damage a champion has taken in total
- Vision Score: How much influence a player has dealt in terms of granting and denying vision
- CS: How many minions, jungle, or objective monsters killed
- Kills: How many kills a champion got
- Deaths: How many deaths a champion got
- Assist: How many assists a champion got
- Objectives: Dragons, Herlands, Barons, Towers, Inhibitors

## Cleaning and EDA

| gameid                | teamname                      | is_malphite   | champion   |   result |   kills |   deaths |   assists |   damagetochampions |   visionscore |   total cs |   totalgold |   damagetaken | Champ Type   |
|:----------------------|:------------------------------|:--------------|:-----------|---------:|--------:|---------:|----------:|--------------------:|--------------:|-----------:|------------:|--------------:|:-------------|
| ESPORTSTMNT01_2690210 | Fredit BRION Challengers      | False         | Renekton   |        0 |       2 |        3 |         2 |               15768 |            26 |        231 |       10934 |         30617 | AD           |
| ESPORTSTMNT01_2690210 | Nongshim RedForce Challengers | False         | Gragas     |        1 |       1 |        1 |        12 |               17455 |            30 |        229 |       10001 |         26034 | AP           |
| ESPORTSTMNT01_2690219 | T1 Challengers                | False         | Gragas     |        0 |       0 |        5 |         2 |                9484 |            28 |        245 |       11076 |         30754 | AP           |
| ESPORTSTMNT01_2690219 | Liiv SANDBOX Challengers      | False         | Gangplank  |        1 |       2 |        2 |         6 |               23632 |            30 |        341 |       17877 |         17752 | AD           |
| 8401-8401_game_1      | Oh My God                     | False         | Gwen       |        1 |       5 |        0 |         4 |               11188 |            23 |        172 |        9123 |         13327 | AP           | 


At this point, we have subset data our anaysis purposes. However, I would like to take one step further and classify each champion based off of the "type" of champion they are.
- For example, Malphite is a Tank Champion, Vayne is an AD (Attack Damage) Champion, and Lilia is an AP (Ability Power) Champion.
- However, this is an subjective classification based off of the typical build path of the Season 12 Meta. 

| gameid                | teamname                      | is_malphite   | champion   |   result |   kills |   deaths |   assists |   damagetochampions |   visionscore |   total cs |   totalgold |   damagetaken | Champ Type   |
|:----------------------|:------------------------------|:--------------|:-----------|---------:|--------:|---------:|----------:|--------------------:|--------------:|-----------:|------------:|--------------:|:-------------|
| ESPORTSTMNT01_2690210 | Fredit BRION Challengers      | False         | Renekton   |        0 |       2 |        3 |         2 |               15768 |            26 |        231 |       10934 |         30617 | AD           |
| ESPORTSTMNT01_2690210 | Nongshim RedForce Challengers | False         | Gragas     |        1 |       1 |        1 |        12 |               17455 |            30 |        229 |       10001 |         26034 | AP           |
| ESPORTSTMNT01_2690219 | T1 Challengers                | False         | Gragas     |        0 |       0 |        5 |         2 |                9484 |            28 |        245 |       11076 |         30754 | AP           |
| ESPORTSTMNT01_2690219 | Liiv SANDBOX Challengers      | False         | Gangplank  |        1 |       2 |        2 |         6 |               23632 |            30 |        341 |       17877 |         17752 | AD           |
| 8401-8401_game_1      | Oh My God                     | False         | Gwen       |        1 |       5 |        0 |         4 |               11188 |            23 |        172 |        9123 |         13327 | AP           |


These are the team level objectives we will be examining:
- Dragons
- Heralds
- Barons
- Towers
- Inhibitors
- Total Objectives (Sum of the 5 above)

| gameid                | teamname                      | is_malphite   | champion   |   result |   kills |   deaths |   assists |   damagetochampions |   visionscore |   total cs |   totalgold |   damagetaken | Champ Type   |   dragons |   heralds |   barons |   towers |   inhibitors |   total objs |
|:----------------------|:------------------------------|:--------------|:-----------|---------:|--------:|---------:|----------:|--------------------:|--------------:|-----------:|------------:|--------------:|:-------------|----------:|----------:|---------:|---------:|-------------:|-------------:|
| ESPORTSTMNT01_2690210 | Fredit BRION Challengers      | False         | Renekton   |        0 |       2 |        3 |         2 |               15768 |            26 |        231 |       10934 |         30617 | AD           |         1 |         2 |        0 |        3 |            0 |            6 |
| ESPORTSTMNT01_2690210 | Nongshim RedForce Challengers | False         | Gragas     |        1 |       1 |        1 |        12 |               17455 |            30 |        229 |       10001 |         26034 | AP           |         3 |         0 |        0 |        6 |            1 |           10 |
| ESPORTSTMNT01_2690219 | T1 Challengers                | False         | Gragas     |        0 |       0 |        5 |         2 |                9484 |            28 |        245 |       11076 |         30754 | AP           |         1 |         1 |        0 |        3 |            0 |            5 |
| ESPORTSTMNT01_2690219 | Liiv SANDBOX Challengers      | False         | Gangplank  |        1 |       2 |        2 |         6 |               23632 |            30 |        341 |       17877 |         17752 | AD           |         4 |         1 |        2 |       11 |            2 |           20 |
| 8401-8401_game_1      | Oh My God                     | False         | Gwen       |        1 |       5 |        0 |         4 |               11188 |            23 |        172 |        9123 |         13327 | AP           |         2 |         0 |        1 |        8 |            1 |           12 |

So to predict the if Malphite has an influence on objective play, we need to narrow down the cases where malphite was played and not played. Then, we select the player level data as we mentioned in the introduction for the two relevant datasets. However, to ensure we can predict the value of the objectives, we need to attribute team level data to the players. But as this is only displayed in the last 2 rows of each subset, we can merge this data based off of the gameid and teamname. But to make sure the merge function does not mess with our dataset, we drop every duplicate based off of gameid or both gameid and teamname.

