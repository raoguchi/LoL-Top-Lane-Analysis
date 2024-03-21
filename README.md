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

<table style ="width:100px; height:200px; border:1px">
<thead>
<tr><th>is_malphite  </th><th>champion  </th><th style="text-align: right;">  result</th><th style="text-align: right;">  kills</th><th style="text-align: right;">  deaths</th><th style="text-align: right;">  assists</th><th style="text-align: right;">  damagetochampions</th><th style="text-align: right;">  visionscore</th><th style="text-align: right;">  total cs</th><th style="text-align: right;">  totalgold</th><th style="text-align: right;">  damagetaken</th></tr>
</thead>
<tbody>
<tr><td>False        </td><td>Renekton  </td><td style="text-align: right;">       0</td><td style="text-align: right;">      2</td><td style="text-align: right;">       3</td><td style="text-align: right;">        2</td><td style="text-align: right;">              15768</td><td style="text-align: right;">           26</td><td style="text-align: right;">       231</td><td style="text-align: right;">      10934</td><td style="text-align: right;">        30617</td></tr>
<tr><td>False        </td><td>Gragas    </td><td style="text-align: right;">       1</td><td style="text-align: right;">      1</td><td style="text-align: right;">       1</td><td style="text-align: right;">       12</td><td style="text-align: right;">              17455</td><td style="text-align: right;">           30</td><td style="text-align: right;">       229</td><td style="text-align: right;">      10001</td><td style="text-align: right;">        26034</td></tr>
<tr><td>False        </td><td>Gragas    </td><td style="text-align: right;">       0</td><td style="text-align: right;">      0</td><td style="text-align: right;">       5</td><td style="text-align: right;">        2</td><td style="text-align: right;">               9484</td><td style="text-align: right;">           28</td><td style="text-align: right;">       245</td><td style="text-align: right;">      11076</td><td style="text-align: right;">        30754</td></tr>
<tr><td>False        </td><td>Gangplank </td><td style="text-align: right;">       1</td><td style="text-align: right;">      2</td><td style="text-align: right;">       2</td><td style="text-align: right;">        6</td><td style="text-align: right;">              23632</td><td style="text-align: right;">           30</td><td style="text-align: right;">       341</td><td style="text-align: right;">      17877</td><td style="text-align: right;">        17752</td></tr>
<tr><td>False        </td><td>Gwen      </td><td style="text-align: right;">       1</td><td style="text-align: right;">      5</td><td style="text-align: right;">       0</td><td style="text-align: right;">        4</td><td style="text-align: right;">              11188</td><td style="text-align: right;">           23</td><td style="text-align: right;">       172</td><td style="text-align: right;">       9123</td><td style="text-align: right;">        13327</td></tr>
</tbody>
</table>


At this point, we have subset data our anaysis purposes. However, I would like to take one step further and classify each champion based off of the "type" of champion they are.
- For example, Malphite is a Tank Champion, Vayne is an AD (Attack Damage) Champion, and Lilia is an AP (Ability Power) Champion.
- However, this is an subjective classification based off of the typical build path of the Season 12 Meta. 

<table style="width:100px; height:200px; border:1px">
<thead>
<tr><th>is_malphite  </th><th>champion  </th><th style="text-align: right;">  result</th><th style="text-align: right;">  kills</th><th style="text-align: right;">  deaths</th><th style="text-align: right;">  assists</th><th style="text-align: right;">  damagetochampions</th><th style="text-align: right;">  visionscore</th><th style="text-align: right;">  total cs</th><th style="text-align: right;">  totalgold</th><th style="text-align: right;">  damagetaken</th><th>Champ Type  </th></tr>
</thead>
<tbody>
<tr><td>False        </td><td>Renekton  </td><td style="text-align: right;">       0</td><td style="text-align: right;">      2</td><td style="text-align: right;">       3</td><td style="text-align: right;">        2</td><td style="text-align: right;">              15768</td><td style="text-align: right;">           26</td><td style="text-align: right;">       231</td><td style="text-align: right;">      10934</td><td style="text-align: right;">        30617</td><td>AD          </td></tr>
<tr><td>False        </td><td>Gragas    </td><td style="text-align: right;">       1</td><td style="text-align: right;">      1</td><td style="text-align: right;">       1</td><td style="text-align: right;">       12</td><td style="text-align: right;">              17455</td><td style="text-align: right;">           30</td><td style="text-align: right;">       229</td><td style="text-align: right;">      10001</td><td style="text-align: right;">        26034</td><td>AP          </td></tr>
<tr><td>False        </td><td>Gragas    </td><td style="text-align: right;">       0</td><td style="text-align: right;">      0</td><td style="text-align: right;">       5</td><td style="text-align: right;">        2</td><td style="text-align: right;">               9484</td><td style="text-align: right;">           28</td><td style="text-align: right;">       245</td><td style="text-align: right;">      11076</td><td style="text-align: right;">        30754</td><td>AP          </td></tr>
<tr><td>False        </td><td>Gangplank </td><td style="text-align: right;">       1</td><td style="text-align: right;">      2</td><td style="text-align: right;">       2</td><td style="text-align: right;">        6</td><td style="text-align: right;">              23632</td><td style="text-align: right;">           30</td><td style="text-align: right;">       341</td><td style="text-align: right;">      17877</td><td style="text-align: right;">        17752</td><td>AD          </td></tr>
<tr><td>False        </td><td>Gwen      </td><td style="text-align: right;">       1</td><td style="text-align: right;">      5</td><td style="text-align: right;">       0</td><td style="text-align: right;">        4</td><td style="text-align: right;">              11188</td><td style="text-align: right;">           23</td><td style="text-align: right;">       172</td><td style="text-align: right;">       9123</td><td style="text-align: right;">        13327</td><td>AP          </td></tr>
</tbody>
</table>


These are the team level objectives we will be examining:
- Dragons
- Heralds
- Barons
- Towers
- Inhibitors
- Total Objectives (Sum of the 5 above)

<table style="width:100px; height:200px; border:1px">
<thead>
<tr><th style="text-align: right;">  </th><th>gameid               </th><th>teamname                     </th><th style="text-align: right;">  dragons</th><th style="text-align: right;">  heralds</th><th style="text-align: right;">  barons</th><th style="text-align: right;">  towers</th><th style="text-align: right;">  inhibitors</th></tr>
</thead>
<tbody>
<tr><td style="text-align: right;">10</td><td>ESPORTSTMNT01_2690210</td><td>Fredit BRION Challengers     </td><td style="text-align: right;">        1</td><td style="text-align: right;">        2</td><td style="text-align: right;">       0</td><td style="text-align: right;">       3</td><td style="text-align: right;">           0</td></tr>
<tr><td style="text-align: right;">11</td><td>ESPORTSTMNT01_2690210</td><td>Nongshim RedForce Challengers</td><td style="text-align: right;">        3</td><td style="text-align: right;">        0</td><td style="text-align: right;">       0</td><td style="text-align: right;">       6</td><td style="text-align: right;">           1</td></tr>
<tr><td style="text-align: right;">22</td><td>ESPORTSTMNT01_2690219</td><td>T1 Challengers               </td><td style="text-align: right;">        1</td><td style="text-align: right;">        1</td><td style="text-align: right;">       0</td><td style="text-align: right;">       3</td><td style="text-align: right;">           0</td></tr>
<tr><td style="text-align: right;">23</td><td>ESPORTSTMNT01_2690219</td><td>Liiv SANDBOX Challengers     </td><td style="text-align: right;">        4</td><td style="text-align: right;">        1</td><td style="text-align: right;">       2</td><td style="text-align: right;">      11</td><td style="text-align: right;">           2</td></tr>
<tr><td style="text-align: right;">34</td><td>8401-8401_game_1     </td><td>Oh My God                    </td><td style="text-align: right;">        2</td><td style="text-align: right;">        0</td><td style="text-align: right;">       1</td><td style="text-align: right;">       8</td><td style="text-align: right;">           1</td></tr>
</tbody>
</table>


So to predict the if Malphite has an influence on objective play, we need to narrow down the cases where malphite was played and not played. Then, we select the player level data as we mentioned in the introduction for the two relevant datasets. However, to ensure we can predict the value of the objectives, we need to attribute team level data to the players. But as this is only displayed in the last 2 rows of each subset, we can merge this data based off of the gameid and teamname. But to make sure the merge function does not mess with our dataset, we drop every duplicate based off of gameid or both gameid and teamname.

<table style="width:100px; height:200px; border:1px">
<thead>
<tr><th>is_malphite  </th><th>champion  </th><th style="text-align: right;">  result</th><th style="text-align: right;">  kills</th><th style="text-align: right;">  deaths</th><th style="text-align: right;">  assists</th><th style="text-align: right;">  damagetochampions</th><th style="text-align: right;">  visionscore</th><th style="text-align: right;">  total cs</th><th style="text-align: right;">  totalgold</th><th style="text-align: right;">  damagetaken</th><th>Champ Type  </th><th style="text-align: right;">  dragons</th><th style="text-align: right;">  heralds</th><th style="text-align: right;">  barons</th><th style="text-align: right;">  towers</th><th style="text-align: right;">  inhibitors</th><th style="text-align: right;">  total objs</th></tr>
</thead>
<tbody>
<tr><td>False        </td><td>Renekton  </td><td style="text-align: right;">       0</td><td style="text-align: right;">      2</td><td style="text-align: right;">       3</td><td style="text-align: right;">        2</td><td style="text-align: right;">              15768</td><td style="text-align: right;">           26</td><td style="text-align: right;">       231</td><td style="text-align: right;">      10934</td><td style="text-align: right;">        30617</td><td>AD          </td><td style="text-align: right;">        1</td><td style="text-align: right;">        2</td><td style="text-align: right;">       0</td><td style="text-align: right;">       3</td><td style="text-align: right;">           0</td><td style="text-align: right;">           6</td></tr>
<tr><td>False        </td><td>Gragas    </td><td style="text-align: right;">       1</td><td style="text-align: right;">      1</td><td style="text-align: right;">       1</td><td style="text-align: right;">       12</td><td style="text-align: right;">              17455</td><td style="text-align: right;">           30</td><td style="text-align: right;">       229</td><td style="text-align: right;">      10001</td><td style="text-align: right;">        26034</td><td>AP          </td><td style="text-align: right;">        3</td><td style="text-align: right;">        0</td><td style="text-align: right;">       0</td><td style="text-align: right;">       6</td><td style="text-align: right;">           1</td><td style="text-align: right;">          10</td></tr>
<tr><td>False        </td><td>Gragas    </td><td style="text-align: right;">       0</td><td style="text-align: right;">      0</td><td style="text-align: right;">       5</td><td style="text-align: right;">        2</td><td style="text-align: right;">               9484</td><td style="text-align: right;">           28</td><td style="text-align: right;">       245</td><td style="text-align: right;">      11076</td><td style="text-align: right;">        30754</td><td>AP          </td><td style="text-align: right;">        1</td><td style="text-align: right;">        1</td><td style="text-align: right;">       0</td><td style="text-align: right;">       3</td><td style="text-align: right;">           0</td><td style="text-align: right;">           5</td></tr>
<tr><td>False        </td><td>Gangplank </td><td style="text-align: right;">       1</td><td style="text-align: right;">      2</td><td style="text-align: right;">       2</td><td style="text-align: right;">        6</td><td style="text-align: right;">              23632</td><td style="text-align: right;">           30</td><td style="text-align: right;">       341</td><td style="text-align: right;">      17877</td><td style="text-align: right;">        17752</td><td>AD          </td><td style="text-align: right;">        4</td><td style="text-align: right;">        1</td><td style="text-align: right;">       2</td><td style="text-align: right;">      11</td><td style="text-align: right;">           2</td><td style="text-align: right;">          20</td></tr>
<tr><td>False        </td><td>Gwen      </td><td style="text-align: right;">       1</td><td style="text-align: right;">      5</td><td style="text-align: right;">       0</td><td style="text-align: right;">        4</td><td style="text-align: right;">              11188</td><td style="text-align: right;">           23</td><td style="text-align: right;">       172</td><td style="text-align: right;">       9123</td><td style="text-align: right;">        13327</td><td>AP          </td><td style="text-align: right;">        2</td><td style="text-align: right;">        0</td><td style="text-align: right;">       1</td><td style="text-align: right;">       8</td><td style="text-align: right;">           1</td><td style="text-align: right;">          12</td></tr>
</tbody>
</table>