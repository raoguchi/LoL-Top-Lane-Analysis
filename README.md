# Is Malphite Broken as a Top Laner?
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

<div style='overflow-x: auto;'>
<table>
<thead>
<tr><th style="text-align: right;">  </th><th>gameid               </th><th>teamname                     </th><th>is_malphite  </th><th>champion  </th><th style="text-align: right;">  result</th><th style="text-align: right;">  kills</th><th style="text-align: right;">  deaths</th><th style="text-align: right;">  assists</th><th style="text-align: right;">  damagetochampions</th><th style="text-align: right;">  visionscore</th><th style="text-align: right;">  total cs</th><th style="text-align: right;">  totalgold</th><th style="text-align: right;">  damagetaken</th></tr>
</thead>
<tbody>
<tr><td style="text-align: right;"> 0</td><td>ESPORTSTMNT01_2690210</td><td>Fredit BRION Challengers     </td><td>False        </td><td>Renekton  </td><td style="text-align: right;">       0</td><td style="text-align: right;">      2</td><td style="text-align: right;">       3</td><td style="text-align: right;">        2</td><td style="text-align: right;">              15768</td><td style="text-align: right;">           26</td><td style="text-align: right;">       231</td><td style="text-align: right;">      10934</td><td style="text-align: right;">        30617</td></tr>
<tr><td style="text-align: right;"> 5</td><td>ESPORTSTMNT01_2690210</td><td>Nongshim RedForce Challengers</td><td>False        </td><td>Gragas    </td><td style="text-align: right;">       1</td><td style="text-align: right;">      1</td><td style="text-align: right;">       1</td><td style="text-align: right;">       12</td><td style="text-align: right;">              17455</td><td style="text-align: right;">           30</td><td style="text-align: right;">       229</td><td style="text-align: right;">      10001</td><td style="text-align: right;">        26034</td></tr>
<tr><td style="text-align: right;">12</td><td>ESPORTSTMNT01_2690219</td><td>T1 Challengers               </td><td>False        </td><td>Gragas    </td><td style="text-align: right;">       0</td><td style="text-align: right;">      0</td><td style="text-align: right;">       5</td><td style="text-align: right;">        2</td><td style="text-align: right;">               9484</td><td style="text-align: right;">           28</td><td style="text-align: right;">       245</td><td style="text-align: right;">      11076</td><td style="text-align: right;">        30754</td></tr>
<tr><td style="text-align: right;">17</td><td>ESPORTSTMNT01_2690219</td><td>Liiv SANDBOX Challengers     </td><td>False        </td><td>Gangplank </td><td style="text-align: right;">       1</td><td style="text-align: right;">      2</td><td style="text-align: right;">       2</td><td style="text-align: right;">        6</td><td style="text-align: right;">              23632</td><td style="text-align: right;">           30</td><td style="text-align: right;">       341</td><td style="text-align: right;">      17877</td><td style="text-align: right;">        17752</td></tr>
<tr><td style="text-align: right;">24</td><td>8401-8401_game_1     </td><td>Oh My God                    </td><td>False        </td><td>Gwen      </td><td style="text-align: right;">       1</td><td style="text-align: right;">      5</td><td style="text-align: right;">       0</td><td style="text-align: right;">        4</td><td style="text-align: right;">              11188</td><td style="text-align: right;">           23</td><td style="text-align: right;">       172</td><td style="text-align: right;">       9123</td><td style="text-align: right;">        13327</td></tr>
</tbody>
</table>
</div>


At this point, we have subset data our anaysis purposes. However, I would like to take one step further and classify each champion based off of the "type" of champion they are.
- For example, Malphite is a Tank Champion, Vayne is an AD (Attack Damage) Champion, and Lilia is an AP (Ability Power) Champion.
- However, this is an subjective classification based off of the typical build path of the Season 12 Meta. 

<div style='overflow-x: auto;'>
<table>
<thead>
<tr><th style="text-align: right;">  </th><th>gameid               </th><th>teamname                     </th><th>is_malphite  </th><th>champion  </th><th style="text-align: right;">  result</th><th style="text-align: right;">  kills</th><th style="text-align: right;">  deaths</th><th style="text-align: right;">  assists</th><th style="text-align: right;">  damagetochampions</th><th style="text-align: right;">  visionscore</th><th style="text-align: right;">  total cs</th><th style="text-align: right;">  totalgold</th><th style="text-align: right;">  damagetaken</th><th>Champ Type  </th></tr>
</thead>
<tbody>
<tr><td style="text-align: right;"> 0</td><td>ESPORTSTMNT01_2690210</td><td>Fredit BRION Challengers     </td><td>False        </td><td>Renekton  </td><td style="text-align: right;">       0</td><td style="text-align: right;">      2</td><td style="text-align: right;">       3</td><td style="text-align: right;">        2</td><td style="text-align: right;">              15768</td><td style="text-align: right;">           26</td><td style="text-align: right;">       231</td><td style="text-align: right;">      10934</td><td style="text-align: right;">        30617</td><td>AD          </td></tr>
<tr><td style="text-align: right;"> 5</td><td>ESPORTSTMNT01_2690210</td><td>Nongshim RedForce Challengers</td><td>False        </td><td>Gragas    </td><td style="text-align: right;">       1</td><td style="text-align: right;">      1</td><td style="text-align: right;">       1</td><td style="text-align: right;">       12</td><td style="text-align: right;">              17455</td><td style="text-align: right;">           30</td><td style="text-align: right;">       229</td><td style="text-align: right;">      10001</td><td style="text-align: right;">        26034</td><td>AP          </td></tr>
<tr><td style="text-align: right;">12</td><td>ESPORTSTMNT01_2690219</td><td>T1 Challengers               </td><td>False        </td><td>Gragas    </td><td style="text-align: right;">       0</td><td style="text-align: right;">      0</td><td style="text-align: right;">       5</td><td style="text-align: right;">        2</td><td style="text-align: right;">               9484</td><td style="text-align: right;">           28</td><td style="text-align: right;">       245</td><td style="text-align: right;">      11076</td><td style="text-align: right;">        30754</td><td>AP          </td></tr>
<tr><td style="text-align: right;">17</td><td>ESPORTSTMNT01_2690219</td><td>Liiv SANDBOX Challengers     </td><td>False        </td><td>Gangplank </td><td style="text-align: right;">       1</td><td style="text-align: right;">      2</td><td style="text-align: right;">       2</td><td style="text-align: right;">        6</td><td style="text-align: right;">              23632</td><td style="text-align: right;">           30</td><td style="text-align: right;">       341</td><td style="text-align: right;">      17877</td><td style="text-align: right;">        17752</td><td>AD          </td></tr>
<tr><td style="text-align: right;">24</td><td>8401-8401_game_1     </td><td>Oh My God                    </td><td>False        </td><td>Gwen      </td><td style="text-align: right;">       1</td><td style="text-align: right;">      5</td><td style="text-align: right;">       0</td><td style="text-align: right;">        4</td><td style="text-align: right;">              11188</td><td style="text-align: right;">           23</td><td style="text-align: right;">       172</td><td style="text-align: right;">       9123</td><td style="text-align: right;">        13327</td><td>AP          </td></tr>
</tbody>
</table>
</div>


These are the team level objectives we will be examining:
- Dragons
- Heralds
- Barons
- Towers
- Inhibitors
- Total Objectives (Sum of the 5 above)

<div style='overflow-x: auto;'>
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
</div>


So to predict the if Malphite has an influence on objective play, we need to narrow down the cases where malphite was played and not played. Then, we select the player level data as we mentioned in the introduction for the two relevant datasets. However, to ensure we can predict the value of the objectives, we need to attribute team level data to the players. But as this is only displayed in the last 2 rows of each subset, we can merge this data based off of the gameid and teamname. But to make sure the merge function does not mess with our dataset, we drop every duplicate based off of gameid or both gameid and teamname.

<div style='overflow-x: auto;'>
<table>
<thead>
<tr><th style="text-align: right;">  </th><th>gameid               </th><th>teamname                     </th><th>is_malphite  </th><th>champion  </th><th style="text-align: right;">  result</th><th style="text-align: right;">  kills</th><th style="text-align: right;">  deaths</th><th style="text-align: right;">  assists</th><th style="text-align: right;">  damagetochampions</th><th style="text-align: right;">  visionscore</th><th style="text-align: right;">  total cs</th><th style="text-align: right;">  totalgold</th><th style="text-align: right;">  damagetaken</th><th>Champ Type  </th><th style="text-align: right;">  dragons</th><th style="text-align: right;">  heralds</th><th style="text-align: right;">  barons</th><th style="text-align: right;">  towers</th><th style="text-align: right;">  inhibitors</th><th style="text-align: right;">  total objs</th></tr>
</thead>
<tbody>
<tr><td style="text-align: right;"> 0</td><td>ESPORTSTMNT01_2690210</td><td>Fredit BRION Challengers     </td><td>False        </td><td>Renekton  </td><td style="text-align: right;">       0</td><td style="text-align: right;">      2</td><td style="text-align: right;">       3</td><td style="text-align: right;">        2</td><td style="text-align: right;">              15768</td><td style="text-align: right;">           26</td><td style="text-align: right;">       231</td><td style="text-align: right;">      10934</td><td style="text-align: right;">        30617</td><td>AD          </td><td style="text-align: right;">        1</td><td style="text-align: right;">        2</td><td style="text-align: right;">       0</td><td style="text-align: right;">       3</td><td style="text-align: right;">           0</td><td style="text-align: right;">           6</td></tr>
<tr><td style="text-align: right;"> 1</td><td>ESPORTSTMNT01_2690210</td><td>Nongshim RedForce Challengers</td><td>False        </td><td>Gragas    </td><td style="text-align: right;">       1</td><td style="text-align: right;">      1</td><td style="text-align: right;">       1</td><td style="text-align: right;">       12</td><td style="text-align: right;">              17455</td><td style="text-align: right;">           30</td><td style="text-align: right;">       229</td><td style="text-align: right;">      10001</td><td style="text-align: right;">        26034</td><td>AP          </td><td style="text-align: right;">        3</td><td style="text-align: right;">        0</td><td style="text-align: right;">       0</td><td style="text-align: right;">       6</td><td style="text-align: right;">           1</td><td style="text-align: right;">          10</td></tr>
<tr><td style="text-align: right;"> 2</td><td>ESPORTSTMNT01_2690219</td><td>T1 Challengers               </td><td>False        </td><td>Gragas    </td><td style="text-align: right;">       0</td><td style="text-align: right;">      0</td><td style="text-align: right;">       5</td><td style="text-align: right;">        2</td><td style="text-align: right;">               9484</td><td style="text-align: right;">           28</td><td style="text-align: right;">       245</td><td style="text-align: right;">      11076</td><td style="text-align: right;">        30754</td><td>AP          </td><td style="text-align: right;">        1</td><td style="text-align: right;">        1</td><td style="text-align: right;">       0</td><td style="text-align: right;">       3</td><td style="text-align: right;">           0</td><td style="text-align: right;">           5</td></tr>
<tr><td style="text-align: right;"> 3</td><td>ESPORTSTMNT01_2690219</td><td>Liiv SANDBOX Challengers     </td><td>False        </td><td>Gangplank </td><td style="text-align: right;">       1</td><td style="text-align: right;">      2</td><td style="text-align: right;">       2</td><td style="text-align: right;">        6</td><td style="text-align: right;">              23632</td><td style="text-align: right;">           30</td><td style="text-align: right;">       341</td><td style="text-align: right;">      17877</td><td style="text-align: right;">        17752</td><td>AD          </td><td style="text-align: right;">        4</td><td style="text-align: right;">        1</td><td style="text-align: right;">       2</td><td style="text-align: right;">      11</td><td style="text-align: right;">           2</td><td style="text-align: right;">          20</td></tr>
<tr><td style="text-align: right;"> 4</td><td>8401-8401_game_1     </td><td>Oh My God                    </td><td>False        </td><td>Gwen      </td><td style="text-align: right;">       1</td><td style="text-align: right;">      5</td><td style="text-align: right;">       0</td><td style="text-align: right;">        4</td><td style="text-align: right;">              11188</td><td style="text-align: right;">           23</td><td style="text-align: right;">       172</td><td style="text-align: right;">       9123</td><td style="text-align: right;">        13327</td><td>AP          </td><td style="text-align: right;">        2</td><td style="text-align: right;">        0</td><td style="text-align: right;">       1</td><td style="text-align: right;">       8</td><td style="text-align: right;">           1</td><td style="text-align: right;">          12</td></tr>
</tbody>
</table>
</div>


## Univariate Analysis

Now it is time to examine how many total objectives are taken on average between the Malphite and No Malphite Groups.

<iframe
  src="assets/boxplot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

It appears here that based that range of total objective tends to differ widely based off of the existence of a Malphite. Overall, the average and maximum of total objectives obtained tends to lean towards the group with no Malphite in the top lane.

## Bivariate Analysis

However, this might not tell the full story as the performance of Malphite might greatly influence the total objectives captured. (This perhaps may just be stating an obvious fact)

<iframe
  src="assets/scatter1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Although the meaning of this data is slightly hard to interpret due to the mass amount of data, there seems to be a reasonable conclusion that I can make. Compared to the rest of the dataset, Malphite does not need ot deal as much damage to enemy champions (or be the main carry) to have an influence on objective play. This average clusters itself around 12k-15k where as the average top laner tends ot deal around 20k+ in champion damage.

<iframe
  src="assets/scatter2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Here is an interpretation based off of champ type. It is quite clear that most players play AD top laners. Althought visually hard to interpret, there seems to a positive correlation between damage and total objectives taken. This can be recognized as the spread tends to incraese as you increase the total objective value.

<table>
<thead>
<tr><th>     </th><th style="text-align: right;">  Champ Type</th></tr>
</thead>
<tbody>
<tr><td>AD   </td><td style="text-align: right;">    0.424585</td></tr>
<tr><td>Tank </td><td style="text-align: right;">    0.251181</td></tr>
<tr><td>AP   </td><td style="text-align: right;">    0.213787</td></tr>
<tr><td>Other</td><td style="text-align: right;">    0.110447</td></tr>
</tbody>
</table>

<table>
<thead>
<tr><th>Champ Type  </th><th style="text-align: right;">  total objs</th></tr>
</thead>
<tbody>
<tr><td>AD          </td><td style="text-align: right;">     10.5944</td></tr>
<tr><td>AP          </td><td style="text-align: right;">     10.2303</td></tr>
<tr><td>Other       </td><td style="text-align: right;">     10.4066</td></tr>
<tr><td>Tank        </td><td style="text-align: right;">     10.0904</td></tr>
</tbody>
</table>

<table>
<thead>
<tr><th>Champ Type  </th><th style="text-align: right;">  damagetochampions</th></tr>
</thead>
<tbody>
<tr><td>AD          </td><td style="text-align: right;">            16185  </td></tr>
<tr><td>AP          </td><td style="text-align: right;">            15846.1</td></tr>
<tr><td>Other       </td><td style="text-align: right;">            15388.2</td></tr>
<tr><td>Tank        </td><td style="text-align: right;">            14233.9</td></tr>
</tbody>
</table>

<table>
<thead>
<tr><th>Champ Type  </th><th style="text-align: right;">  totalgold</th></tr>
</thead>
<tbody>
<tr><td>AD          </td><td style="text-align: right;">    12858.5</td></tr>
<tr><td>AP          </td><td style="text-align: right;">    12060.5</td></tr>
<tr><td>Other       </td><td style="text-align: right;">    11992  </td></tr>
<tr><td>Tank        </td><td style="text-align: right;">    11700.5</td></tr>
</tbody>
</table>

<table>
<thead>
<tr><th>Champ Type  </th><th style="text-align: right;">  heralds</th></tr>
</thead>
<tbody>
<tr><td>AD          </td><td style="text-align: right;"> 0.819574</td></tr>
<tr><td>AP          </td><td style="text-align: right;"> 0.775406</td></tr>
<tr><td>Other       </td><td style="text-align: right;"> 0.848995</td></tr>
<tr><td>Tank        </td><td style="text-align: right;"> 0.805466</td></tr>
</tbody>
</table>

Between the two charts above, there acutally seems to be only a small, positive correlation between the amount of total objectives and top laners. Assuming the AD top laners are the meta due to the overwhelming pick rate, we would expect there can be a key reason to do so.

- Damage wise, we would expect AD champs to be ahead of tanks, this seems to hold true.
- However, based off of objectives taken, there is no noticeable change.
- As for gold, there is there is a range of 1100, which is actually not enough to buy one item in the game.
- However, there seems to be a noticable difference in average Heralds per game. This can most likely be attributed to the AD laner's ability to be more impactful in Skirmishes. As noticed in the lower mean value in AP champs, they are relatively weak during the 9:50 minutue mark, which indicates that it is probably better to have Tanks or AD champs in the early game.

## Assessment of Missingness

Missingness Analysis: Before, the game_data dataframe had Missing Values for certain objectives that I imputed with zeros. This is assuming that the original dataset considered missing values have zeros. Along these lines, we are assuming these are missing by design. However, now we are going to see if missingness has any correlation between the type of champ a laner plays. We will be specifically looking at the Rift Herald as this spawns during the laning phase at 9.5 minutes.

<table>
<thead>
<tr><th style="text-align: right;">  </th><th>Champ Type  </th><th style="text-align: right;">  heralds</th><th>heralds_missing  </th></tr>
</thead>
<tbody>
<tr><td style="text-align: right;"> 0</td><td>AD          </td><td style="text-align: right;">        2</td><td>False            </td></tr>
<tr><td style="text-align: right;"> 1</td><td>AP          </td><td style="text-align: right;">        0</td><td>False            </td></tr>
<tr><td style="text-align: right;"> 2</td><td>AP          </td><td style="text-align: right;">        1</td><td>False            </td></tr>
<tr><td style="text-align: right;"> 3</td><td>AD          </td><td style="text-align: right;">        1</td><td>False            </td></tr>
<tr><td style="text-align: right;"> 4</td><td>AP          </td><td style="text-align: right;">      nan</td><td>True             </td></tr>
</tbody>
</table>

<table>
<thead>
<tr><th>Champ Type  </th><th style="text-align: right;">  heralds_missing = False</th><th style="text-align: right;">  heralds_missing = True</th></tr>
</thead>
<tbody>
<tr><td>AD          </td><td style="text-align: right;">                 0.428847</td><td style="text-align: right;">                0.40534 </td></tr>
<tr><td>AP          </td><td style="text-align: right;">                 0.20398 </td><td style="text-align: right;">                0.25673 </td></tr>
<tr><td>Other       </td><td style="text-align: right;">                 0.109637</td><td style="text-align: right;">                0.114467</td></tr>
<tr><td>Tank        </td><td style="text-align: right;">                 0.257536</td><td style="text-align: right;">                0.223462</td></tr>
</tbody>
</table>

Overall, the missingness seems to be roughly the same across the type of champ. However, there is an underlying difference between the AP champs having a missing value for taking the rift herald. Hence, there is a need to test if the missiness is Not Missing at Random or Missing Completely at Random. I believe that the missingness of the herald is actually dependent on the champ type. This is because AP champs have low combat impact in early states of the game compared to AD/Tank laners and junglers.

After conducting our permutation analysis, p value was 0, and observed TVD was 0.06, we have reason to state that the missingness is actually dependent on the type of top lane champion. This can be supported with the distribution graph below. Hence, we are going to claim missingness here is Missing at Random. 

<iframe
  src="assets/TVD1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Alternatively, we will check if Missingness will be dependent on a more gameplay based data such as the amount of deaths.

<table>
<thead>
<tr><th style="text-align: right;">  deaths</th><th style="text-align: right;">  heralds_missing = False</th><th style="text-align: right;">  heralds_missing = True</th></tr>
</thead>
<tbody>
<tr><td style="text-align: right;">       0</td><td style="text-align: right;">              0.100827   </td><td style="text-align: right;">             0.0906106  </td></tr>
<tr><td style="text-align: right;">       1</td><td style="text-align: right;">              0.159531   </td><td style="text-align: right;">             0.16021    </td></tr>
<tr><td style="text-align: right;">       2</td><td style="text-align: right;">              0.188982   </td><td style="text-align: right;">             0.190633   </td></tr>
<tr><td style="text-align: right;">       3</td><td style="text-align: right;">              0.185369   </td><td style="text-align: right;">             0.187568   </td></tr>
<tr><td style="text-align: right;">       4</td><td style="text-align: right;">              0.15666    </td><td style="text-align: right;">             0.152331   </td></tr>
<tr><td style="text-align: right;">       5</td><td style="text-align: right;">              0.104786   </td><td style="text-align: right;">             0.111841   </td></tr>
<tr><td style="text-align: right;">       6</td><td style="text-align: right;">              0.0574172  </td><td style="text-align: right;">             0.0577807  </td></tr>
<tr><td style="text-align: right;">       7</td><td style="text-align: right;">              0.0284611  </td><td style="text-align: right;">             0.0286715  </td></tr>
<tr><td style="text-align: right;">       8</td><td style="text-align: right;">              0.011335   </td><td style="text-align: right;">             0.0133508  </td></tr>
<tr><td style="text-align: right;">       9</td><td style="text-align: right;">              0.00376182 </td><td style="text-align: right;">             0.00503392 </td></tr>
<tr><td style="text-align: right;">      10</td><td style="text-align: right;">              0.00173242 </td><td style="text-align: right;">             0.00175093 </td></tr>
<tr><td style="text-align: right;">      11</td><td style="text-align: right;">              0.000742464</td><td style="text-align: right;">             0.000218866</td></tr>
<tr><td style="text-align: right;">      12</td><td style="text-align: right;">              0.000247488</td><td style="text-align: right;">             0          </td></tr>
<tr><td style="text-align: right;">      13</td><td style="text-align: right;">              9.89952e-05</td><td style="text-align: right;">             0          </td></tr>
<tr><td style="text-align: right;">      14</td><td style="text-align: right;">              4.94976e-05</td><td style="text-align: right;">             0          </td></tr>
</tbody>
</table>

After conducting our permutation analysis, p value was 0.704, and observed TVD was 0.015, we have reason to state that the missingness is not dependent on the amount of deaths. This can be supported with the distribution graph below. Hence, we are going to claim missingness here Missing Completely at Random. 

<iframe
  src="assets/TVD2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Alternatively, we see here that herald capture rate is not dependent on the amount of deaths a champions may have. Logically speaking, we may assume poor performance means there is a smaller likelihood of capturing objectives. However, these deaths may occur when the game exits the laning phase. Hence, there is a sound argument to believe that herald captures is not dependent on the deaths a laner may have.
















