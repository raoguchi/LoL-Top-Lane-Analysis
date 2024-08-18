


## Is Malphite Broken as a Top Laner? (What are the necessary Top Lane Win Conditions?)
by Ryosuke (Alex) Oguchi

## Introduction

The dataset we are examining here is the 2022 data for League of Legends Esports Matches. The dataset is subset in groups of 12, which the corresponding match ID. However, as there are only 10 players in each match (5 per team), there are 10 rows about player data and 2 rows about team data. When I claim a champ is "broken", there are various metrics we can use to test this claim. Since this is examining perfromance based data on champions/players, any objective based data will not be part of our model. Moreover, objective based data is a team performance statistic that is "Missing by Design" for individual statistics. Nevertheless, we can assume good top laner performance will contribute to the overall objective completion for each team. Although this may be contingent on the other player's performance (especially the jungler), lane pressure will create a situation where the wave is shoved in due to power difference between the two players. This allows the jungler to take claim to the Rift Herald which is directly related to taking towers.

Just as a discretion, Malphite's build path tends to take one of two paths: Ability Power and Tank. As the latter suggests, the Tank build focuses on building up Damage Resistance by sacrificing damage output. For our purposes, we will take Malphite to be a tank as this is what many players built during the Season 12 meta.

As described in the official League of Legends Website, it is described as follows: "UNSTOPPABLE FORCE: Malphite launches himself to a location at high speed, damaging enemies and knocking them into the air."

Because this move is quite literally unstoppable, it has the power to influence individual match-ups and team fights into one-sided affairs.

<iframe width="560" height="315" 
src="https://youtu.be/Rgta7WDz3JE?si=O4Yw_rGTQIysFXpz" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen>
</iframe>


Now that I have established Malphite is not fair, it is necessary to narrow down what type of Data is relevant for analysis. What we are trying to answer is what is the predicted amount of total objectives (dragons, heralds, barons, towards, inhibitors, etc.) that a team will get based off the existence of an "effective" Malphite?

As for individual statistics, we will examine, damage, damage taken, vision score, cs, kills, deaths, assists, and objectives.

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


So to predict the if Malphite has an influence on objective play, we need to narrow down the cases where malphite was played and not played. Then, we select the player level data as we mentioned in the introduction for the two relevant datasets. However, to ensure we can view the total objective count, we need to attribute team level data to the players. But as this is only displayed in the last 2 rows of each subset, we can merge this data based off of the gameid and teamname. But to make sure the merge function does not mess with our dataset, we drop every duplicate based off of gameid or both gameid and teamname.

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

Although the meaning of this data is slightly hard to interpret due to the mass amount of data, there seems to be a reasonable conclusion that I can make. Compared to the rest of the dataset, Malphite does not need to deal as much damage to enemy champions (or be the main carry) to have an influence on objective play. This average clusters itself around 12k-15k where as the average top laner tends to deal around 20k+ in champion damage.

<iframe
  src="assets/scatter2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Here is an interpretation based off of champ type. It is quite clear that most players play AD top laners. Althought visually hard to interpret, there seems to a positive correlation between damage and total objectives taken. This can be recognized as the spread tends to increase as you increase the total objective value.

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
  src="assets/TVD.html"
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

## Hypothesis Testing

Returning back to the original question, we are here to determine if picking Malphite results in better game preformance. In order to test this, we are going to be examining the total objectives taken.

Null Hypothesis: In the population, the total objectives counts for Malphite and Non-Malphite Players will have the same distribution, and differences will be due to random chance.

Alternative Hypothesis: In the population, the total objectives counts for Malphite and Non-Malphite Players not be the same. If anything, the Malphite players will have a different score.

Test Statistic: We will use differnce in group means with 1000 repetitions.

<table>
<thead>
<tr><th style="text-align: right;">  </th><th>is_malphite  </th><th style="text-align: right;">  total objs</th></tr>
</thead>
<tbody>
<tr><td style="text-align: right;"> 0</td><td>False        </td><td style="text-align: right;">           6</td></tr>
<tr><td style="text-align: right;"> 1</td><td>False        </td><td style="text-align: right;">          10</td></tr>
<tr><td style="text-align: right;"> 2</td><td>False        </td><td style="text-align: right;">           5</td></tr>
<tr><td style="text-align: right;"> 3</td><td>False        </td><td style="text-align: right;">          20</td></tr>
<tr><td style="text-align: right;"> 4</td><td>False        </td><td style="text-align: right;">          12</td></tr>
</tbody>
</table>

After conducting 1000 repetitions of our permutation, our observed difference was -1.05. 

<table>
<thead>
<tr><th>is_malphite  </th><th style="text-align: right;">  total objs</th></tr>
</thead>
<tbody>
<tr><td>False        </td><td style="text-align: right;">    10.3821 </td></tr>
<tr><td>True         </td><td style="text-align: right;">     9.33003</td></tr>
</tbody>
</table>

Overall, the observed difference tends to be a rare occurence. This is seen as our p_value is 0.004 (basically zero). By testing at a significance of 5 percent and 2 tails, the p_value has significance. Hence, we must reject the null hypothesis and claim that Malphite the distribution looks different from their counterparts. However, this is not an outright claim. This is because there may be different factors that Malphite players have that result in the overall gameplay disadvantage.

## Framing a Prediction Problem

At this point, there can only be few conclusion that can be made. We can see that picking Malphite leads to lower objective performance. However, now it is time to see we can predict an overall win or loss based off of based off of certain top laner characteristics through a binary classification model (win/lose).

We deliberately chose to ignore KDA (Kills Deaths Assists for future reference) and total gold as they have correlation between Damage to Champions, Damage Taken, and/or Total CS. To evaluate the performance of this model, we will be looking at the accuracy of this model; this is because we are attempting to see if model is viable for purposes beyond post-game analysis. Moreover, this may reveal if there are any specific determinants of performance beyond individual performance. For example, we may reveal here that vision may be a greater determinant of objective taking or wave management; nevertheless, we are looking to see if there is other rationale to winning games beyond just killing. If we find that our model is accurate, now there might be an argument that the human element of playing league of legends is less than what we assume. In other words, we are attempting to see if LoL is a matchup based game that only needs certain conditions for victory.

## Baseline Model

For our baseline model, we will look simply at Damage to Champs and Champ Type as our exogeneous variables predict how the overall result of the game.

- The Damage to Champs column will remain as is because it is a quantitative, continous variable.
- Alternatively, the Champ Type will be One Hot Encoded as it is a categorical nominal variable.

To view the performance, we will look at the accuracy of the model and compare it to the the actual data vs predicted values from out classification model.

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

<iframe src="assets/confusion_matrix.png" width="800" height="600" frameborder="0"></iframe>>


We see here is that the model we created does not have good accuracy as it is about 60 percent between the train data and the testing data. Moreover, looking at the predictions that where generated by the testing data, we see that the result prediction is very far off (about 10 percent). We hope that adding multiple features here will result in a better accuracy.

As per our confusion matrix, there are many False Positives that hurt our precision in predicting result of our games. Hence, there needs to be more complexity; this can be generated through more features or introducing hyperparameters.


## Final Model

Top pivot from our baseline model, the new features we are considering is the following:

- Vision Score,
- Total CS
- Damage Taken
- Dragons Slayed
- Heralds Slayed
- Baron Slayed


To reason with my I did not use towers, inhibitors, or total objectives is because these will provide uninformative result. The objective of playing a game in league of legends is to destroy the other team's "nexus" or core. The nexus is protected by 3 outer towers, 3 inner towers, 3 inhibitor towers, 3 inhibitors, and 2 nexus towers. Therefore, if a team has a high towers destroyed or inhibitor's destroyed count, it is most likely that these teams will win.

As for the hyperparameters, this will be tuned based off of the available hyperparamters in a RandomForestClassifier. I have specifically chosen criterion, min_split, and depth as this will create 140 unique combinations that the model will look to choose from.

- The idea behind criterion is that GridSearch uses gini as its default form. The simple change to entropy is not meant to reveal something new, it is a mere safeguard as there is an off chance that accuracy will improve.
- The minimum number of samples is being adjusted to test against model variance. The greater the value, the lower the variance the model will have.
- As for the max_depth, it is about creating more splits as the depth increases. So if there are any points of overlaps, I believe that small shifts in the data will be more accurately classified.

<table>
<thead>
<tr><th style="text-align: right;">  </th><th style="text-align: right;">       0</th></tr>
</thead>
<tbody>
<tr><td style="text-align: right;"> 1</td><td style="text-align: right;">0.529982</td></tr>
<tr><td style="text-align: right;"> 0</td><td style="text-align: right;">0.470018</td></tr>
</tbody>
</table>

On first glance, the distribution of wins/losses are where the model needs to be. There is about a percent margin of error, but that is much better in terms of the baseline model. As for your accuracy, the trees appears to be overfitting to our training data as with most cases of classifier models. Nevertheless, there is about a 30 percent boost in both the training and testing accuracy. Moreover, we see the accuracy between the training and testing datasets are about 3-4 percent so we are going to say performance on unseen data will not be as bad.

<iframe src="assets/cm2.png" width="800" height="600" frameborder="0"></iframe>

Relative to the confusion matrix in the baseline Model, we see there is a much greater number of true positives and true negatives. This is also associated with greater accuracy. The key issue in the base model is that it contained a fair amount of false positives that harmed the precision. Here, as the relative amount of false positives and false negatives are equal, the recall and preicison is also close to being equal.

## Fairness Analysis

To evaluate this model, we are going to see if this model is sensitive to damage heavy players. Top lane gameplay is often described as an "island" as they are excluded from the actions of other players. Therefore, wave management and trading kills are the keys to ensure they have an impact later on in the game. So by taking the mean damage of the threshold, we are going to consider people who do little damage as below 15k and many damange as above.

Specifically, we will be looking at the false discovery rate as this is the proportion of players we predicted to win when they would not have won.

The false discovery rate is found to be 12.61 percent.

<div style='overflow-x: auto;'>
<table>
<thead>
<tr><th style="text-align: right;">     </th><th style="text-align: right;">  damagetochampions</th><th>Champ Type  </th><th style="text-align: right;">  visionscore</th><th style="text-align: right;">  total cs</th><th style="text-align: right;">  damagetaken</th><th style="text-align: right;">  dragons</th><th style="text-align: right;">  heralds</th><th style="text-align: right;">  barons</th><th style="text-align: right;">  prediction</th><th style="text-align: right;">  testing_result</th><th>high_damage  </th></tr>
</thead>
<tbody>
<tr><td style="text-align: right;">15498</td><td style="text-align: right;">              13581</td><td>Tank        </td><td style="text-align: right;">           33</td><td style="text-align: right;">       232</td><td style="text-align: right;">        23604</td><td style="text-align: right;">        3</td><td style="text-align: right;">        0</td><td style="text-align: right;">       1</td><td style="text-align: right;">           1</td><td style="text-align: right;">               0</td><td>high         </td></tr>
<tr><td style="text-align: right;"> 2880</td><td style="text-align: right;">              14788</td><td>AD          </td><td style="text-align: right;">           17</td><td style="text-align: right;">       260</td><td style="text-align: right;">        22553</td><td style="text-align: right;">        3</td><td style="text-align: right;">        2</td><td style="text-align: right;">       2</td><td style="text-align: right;">           1</td><td style="text-align: right;">               1</td><td>high         </td></tr>
<tr><td style="text-align: right;">20236</td><td style="text-align: right;">              14215</td><td>Other       </td><td style="text-align: right;">           18</td><td style="text-align: right;">       210</td><td style="text-align: right;">        32599</td><td style="text-align: right;">        0</td><td style="text-align: right;">        0</td><td style="text-align: right;">       0</td><td style="text-align: right;">           0</td><td style="text-align: right;">               0</td><td>high         </td></tr>
<tr><td style="text-align: right;"> 6230</td><td style="text-align: right;">              15519</td><td>AD          </td><td style="text-align: right;">           48</td><td style="text-align: right;">       344</td><td style="text-align: right;">        49149</td><td style="text-align: right;">        2</td><td style="text-align: right;">        1</td><td style="text-align: right;">       2</td><td style="text-align: right;">           0</td><td style="text-align: right;">               1</td><td>high         </td></tr>
<tr><td style="text-align: right;">19783</td><td style="text-align: right;">              19897</td><td>Other       </td><td style="text-align: right;">           15</td><td style="text-align: right;">       177</td><td style="text-align: right;">        20976</td><td style="text-align: right;">        3</td><td style="text-align: right;">        0</td><td style="text-align: right;">       1</td><td style="text-align: right;">           1</td><td style="text-align: right;">               1</td><td>low          </td></tr>
</tbody>
</table>
</div>

We are going to view this fairness by viewing the predicted result from our testing data.


Below is the win rate for based off of our chosen binary variable.
<table>
<thead>
<tr><th>high_damage  </th><th style="text-align: right;">  prediction</th></tr>
</thead>
<tbody>
<tr><td>high         </td><td style="text-align: right;">    0.403373</td></tr>
<tr><td>low          </td><td style="text-align: right;">    0.629805</td></tr>
</tbody>
</table>

Below is the discovery rate for based off of our chosen binary variable.
<table>
<thead>
<tr><th>high_damage  </th><th style="text-align: right;">  discovery</th></tr>
</thead>
<tbody>
<tr><td>high         </td><td style="text-align: right;">  0.0938159</td></tr>
<tr><td>low          </td><td style="text-align: right;">  0.170859 </td></tr>
</tbody>
</table>

The observed discovery rate here is 7.7 percent; now by conducting a hypothesis test, we are going to examine if there is statistical significance.

<iframe
  src="assets/fairness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

With this test, our Null Hypothesis is that the model is fair and the False Discovery rate is the same for both groups. And the unfair case is when the false discovery rate is different. 

As we see in the graph above, the false discovery rate of 6 percent (with 1 percent margins) has statistical significance. Taking a signifiance level of 5% or 2.5% on each side, we see the p value is extremely small at 0. Therefore, we reject the null hypothesis. Or in other words, our model here is not fair for low and high damage top laners.
