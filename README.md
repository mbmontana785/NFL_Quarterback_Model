# NFL_Quarterback_Model

## An overall look at the 2025 rookie QB class

Before we get into the comps, let's get a general sense of the overall strength of the 2025 NFL rookie quarterback class. This swarmplot falls in line with the general consensus that this is not a great quarterback class. The green dots represent every quarterback in the dataset that the model was trained on. These are quarterbacks whose careers began in 2005 or later. QB_score is our target variable. That's explained in more detail in the notebooks. The red dots represent the top 14 rookie quarterbacks, without names for now.<br>

With this in mind, we know that we shouldn't expect any of these rookies to be compared to any great quarterbacks who have played in the NFL. 

![My Cool Chart](qb_with_rookies.png)

## Not just any QB can get a team to the Super Bowl

The blue dots represent quarterbacks in our dataset (careers beginning in 2005 or later) who have won at least one Super Bowl. The red dots represent quarterbacks who have reached at least one Super Bowl without winning one. The black dots represent the rest of the quarterbacks. 

![My Cool Chart](qb_super_bowls.png)

## Feature glossary
(feature names followed by '_4' indicate last year of college. Feature names followed by '_3' indicate second-to-last year of college. Feature names followed by '_Diff' indicate the difference between the two years, indicating if there was improvement)
* Power 5: Binary indicator. Did the quarterback play in a Power 5 conference?
* QBR: (from ESPN) Adjusted Total Quarterback Rating, which values the quarterback on all play types on a 0-100 scale adjusted for the strength of opposing defenses faced.
* PAA: (from ESPN) Number of points contributed by a quarterback, accounting for QBR and how much he plays, above the level of an average quarterback.
* PLAYS: (from ESPN) Number of points contributed by a quarterback, accounting for QBR and how much he plays, above the level of an average quarterback.
* EPA: (from ESPN) Total expected points added with low leverage plays, according to ESPN Win Probability model, down-weighted.
* PASS: (from ESPN) Expected points added on pass attempts with low leverage plays down-weighted.
* RUN: (from ESPN) Clutch-weighted expected points added through rushes.
* SACK (from ESPN): Expected points added on sacks with low leverage plays down-weighted.
* PEN (from ESPN): Expected points added on penalties with low leverage plays down-weighted.
* RAW (from ESPN): Raw Total Quarterback Rating, which values quarterback on all play types on a 0-100 scale (not adjusted for opposing defenses faced).
* ht: Height (in inches)
* wt: Weight (in pounds)
* forty: 40-yard dash time at the NFL Scouting Combine (in seconds)
* vertical: Vertical leap at the NFL Scouting Combine (in inches)
* broad_jump: Broad jump at the NFL Scouting Combine (in inches)
* cone: Cone drill time at the NFL Scouting Combine (in seconds)
* shuttle: Shuttle time at the NFL Scouting Combine (in seconds)

## Correlation heatmap

![My Cool Chart](correlation_heatmap_QB_success.png)

## Target variable formula (how we calculated QB_score)

(Win Probability Added represents the percentage by which a quarterback improves his team's probability of winning the game on each play. Negative if he hurts his team's winning probability.)

0.1 * Total Win Probability Added for career +
0.3 * WPA per season +
0.2 * Total career touchdown passes + 
0.1 * TD passes per season +
0.2 * Total All-Pro selections +
0.1 * All-Pro selections per season

## Model performance

| Model                  | Optimal alpha    | Optimal L1 | Test R²   | RMSE   |
|------------------------|------------------|------------|-----------|--------|
| Linear Regression      | NA               | NA         | -0.0877   | 0.5766 |
| Lasso Regression       | 0.0399           | NA         | 0.1114    | 0.5212 |
| Ridge Regression       | 100              | NA         | 0.0936    | 0.5264 |
| Elastic Net Regression | 0.2812           | 0.1        | **0.1288**    | **0.5160** |
| Random Forest          | NA               | NA         | -0.2126   | 0.6088 |
| Gradient Boost         | NA               | NA         | -0.2682   | 0.6226 |
| XG Boost               | NA               | NA         | -0.4477   | 0.6652 *(Not CVed)* |

### Features Retained by Elastic Net

| Power 5_3  | PAA_3   | PASS_4  | PEN_3   | vertical|
| Power 5_4  | PAA_4   | RUN_3   | PEN_4   |         |
| QBR_3      | PLAYS_4 | RUN_4   | ht      |         |
| QBR_4      | EPA_4   | SACK_3  | wt      |         |

## Test set results (highlights and lowlights)
Our model's test set contained 65 samples, about 20 percent of the 325-sample dataset. Most of the quarterback data was used to train the model, where the model saw all the variables and the QB_score for each quarterback. Then we showed the model the variables for these 65 quarterbacks, without the QB_score, and the model made predictions.

### Prediction Results (All 65 QBs)

| Name                  | Actual     | Predicted  | Residual   |
|-----------------------|------------|------------|------------|
| Jalen Hurts           | 1.0009     | 0.6678     | 0.3331     |
| Collin Klein          | -0.5784    | 0.6247     | -1.2031    |
| Deshaun Watson        | 1.0312     | 0.5173     | 0.5138     |
| Vince Young           | 0.3953     | 0.4672     | -0.0719    |
| Caleb Williams        | -0.0686    | 0.4613     | -0.5298    |
| Mac Jones             | 0.3206     | 0.4282     | -0.1076    |
| Case Keenum           | 0.3390     | 0.4264     | -0.0874    |
| Dwayne Haskins        | -0.2191    | 0.4151     | -0.6342    |
| Jordan Lynch          | -0.5784    | 0.4045     | -0.9829    |
| E.J. Manuel           | 0.0066     | 0.3912     | -0.3846    |
| Russell Wilson        | 2.1655     | 0.3821     | 1.7834     |
| Landry Jones          | 0.0783     | 0.3265     | -0.2482    |
| Hendon Hooker         | 0.0092     | 0.2828     | -0.2736    |
| Teddy Bridgewater     | 0.4936     | 0.2728     | 0.2207     |
| Sam Howell            | 0.0333     | 0.2587     | -0.2253    |
| Cole McDonald         | -0.5784    | 0.2383     | -0.8167    |
| John Wolford          | -0.0552    | 0.2195     | -0.2747    |
| Colt McCoy            | 0.0565     | 0.2182     | -0.1617    |
| Daniel Jones          | 0.4185     | 0.2159     | 0.2026     |
| Ryan Finley           | -0.2034    | 0.1729     | -0.3762    |
| Colin Kaepernick      | 0.6009     | 0.1556     | 0.4452     |
| Connor Cook           | -0.1138    | 0.1398     | -0.2536    |
| Zach Mettenberger     | -0.1978    | 0.1234     | -0.3213    |
| Nick Fitzgerald       | -0.5784    | 0.1032     | -0.6817    |
| Aaron Murray          | -0.5784    | 0.0510     | -0.6294    |
| DeShone Kizer         | -0.3491    | 0.0334     | -0.3825    |
| Stephen McGee         | 0.0288     | 0.0318     | -0.0030    |
| Ryan Fitzpatrick      | 0.9757     | 0.0218     | 0.9539     |
| Brock Purdy           | 1.3248     | 0.0102     | 1.3146     |
| Mike White            | 0.0231     | 0.0016     | 0.0215     |
| Pat White             | -0.0461    | -0.0029    | -0.0432    |
| Kellen Clemens        | -0.0584    | -0.0363    | -0.0221    |
| Riley Ferguson        | -0.5784    | -0.0502    | -0.5282    |
| Drew Stanton          | 0.0403     | -0.0613    | 0.1016     |
| Carson Strong         | -0.5784    | -0.0647    | -0.5138    |
| Michael Pratt         | -0.5784    | -0.0655    | -0.5129    |
| Luke Falk             | -0.1506    | -0.0716    | -0.0790    |
| Spencer Rattler       | -0.2213    | -0.0760    | -0.1453    |
| Trevor Knight         | -0.5784    | -0.0760    | -0.5024    |
| Sean Mannion          | -0.0434    | -0.0903    | 0.0469     |
| Connor Halliday       | -0.5784    | -0.1046    | -0.4738    |
| Dustin Crum           | -0.5784    | -0.1288    | -0.4496    |
| Jordan Jefferson      | 0.0155     | -0.1343    | 0.1498     |
| Kent Smith            | -0.5784    | -0.1371    | -0.4414    |
| Tom Savage            | -0.1141    | -0.1524    | 0.0384     |
| Andy Dalton           | 1.1444     | -0.1587    | 1.3031     |
| Scott Tolzien         | -0.0393    | -0.1636    | 0.1243     |
| Austin Davis          | -0.1618    | -0.1686    | 0.0067     |
| Trent Edwards         | -0.0813    | -0.1889    | 0.1076     |
| Tommy DeVito          | -0.0979    | -0.1966    | 0.0987     |
| Alex Brink            | -0.5784    | -0.2500    | -0.3284    |
| Jordan Love           | 0.6116     | -0.2524    | 0.8640     |
| Christian Hackenberg  | -0.5784    | -0.2568    | -0.3216    |
| Anthony Morelli       | -0.5784    | -0.2619    | -0.3166    |
| Drew Willy            | -0.5784    | -0.2621    | -0.3163    |
| Brady White           | -0.5784    | -0.2712    | -0.3072    |
| Peyton Ramsey         | 0.0198     | -0.2727    | 0.2925     |
| Rhett Bomar           | -0.5784    | -0.2956    | -0.2828    |
| Jevan Snead           | -0.5784    | -0.3389    | -0.2396    |
| Thaddeus Lewis        | -0.0280    | -0.3644    | 0.3365     |
| Charlie Frye          | -0.2072    | -0.3947    | 0.1874     |
| James Pinkney         | -0.5784    | -0.3954    | -0.1830    |
| Quinton Porter        | -0.5784    | -0.4024    | -0.1760    |
| Jake Haener           | -0.0688    | -0.4101    | 0.3413     |
| Kyle Wright           | -0.5784    | -0.6133    | 0.0349     |

### Highlights and lowlights.

## Coming up
* Show test set predictions dataframe (highlights and lowlights)
* Show comps
* Show dataset ranked by QB_score
