# League of Legends 2022 Esports Top Lane Analysis
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

As for individual statistics, we will examine, the damage charts, damage taken charts, the vision score charts, the cs charts, the kills charts, the deaths charts, and the assist charts.

Damage to Champions: How much damage a champion has dealt to other champions in total
Damage Taken: How much damage a champion has taken in total
Vision Score: How much influence a player has dealt in terms of granting and denying vision
CS: How many minions, jungle, or objective monsters killed
Kills: How many kills a champion got
Deaths: How many deaths a champion got
Assist: How many assists a champion got

## Cleaning and EDA

