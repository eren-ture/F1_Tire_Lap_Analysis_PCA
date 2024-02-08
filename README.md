# F1 Tire/Lap Analysis with PCA
## Data

F1 Data containing lap information for races in the 2015-2016 season.

Data acquired from: https://github.com/mvmonaghan/f1-tires/tree/master

## Motivation

Explain the core reasons of choosing a tire in a lap.

## Data Pre-processing

- To account for different amount of laps between races, added the `PERCENT_COMPLETE` feature. =(`LAP`/`LAPS`)
- Normalized `TIME` and `GAP` per race, to account for different lap times. =((`X`-`mean`)/`std`) per race
- Removed laps where the driver is lapped, as they have no `GAP` data.
- Removed anomalies:
  - Laps where the driver is 2 standard deviations slower. (Laps where there is driver error or a yellow flag.)
  - Laps where wet tires are used.
  - Out-laps.

The Analysis is done with these **5 features**:
- `STINT LAP`: How many laps the current tire's been used.
- `PERCENT_COMPLETE`: The percent of race completed.
- `TIME_DELTA`: Normalized time data.
- `GAP_DELTA`: Normalized time gap data to the race leader.
- `TIRE`: The tire compund used in that lap.

## Analysis of All Races in the Dataset
### Visualizations

In the pair plot of the features in all races, the tires are very overlapped.

![](https://github.com/eren-ture/F1_Tire_Lap_Analysis_PCA/blob/main/figures/All_Races/pair_plot_beforePCA.png)

There is the obvious relations between `PERCENT_COMPLETE` and `STINT_LAP`,

and a less obvious relation between `TIME_DELTA` and `PERCENT_COMPLETE`, that becomes more apparent with a density graph:

![](https://github.com/eren-ture/F1_Tire_Lap_Analysis_PCA/blob/main/figures/All_Races/time%3Aperc_comp.png)

### PCA

Plotting the **covariance matrix** of the data, the relations can be confirmed.

![](https://github.com/eren-ture/F1_Tire_Lap_Analysis_PCA/blob/main/figures/All_Races/COV_Matrix.png)

Also, by plotting the **percent variances explained**, we see that 90% of the data can be explained by the first 3 values.

![](https://github.com/eren-ture/F1_Tire_Lap_Analysis_PCA/blob/main/figures/All_Races/eigenvalues.png)

**Eigenvectors**

![](https://github.com/eren-ture/F1_Tire_Lap_Analysis_PCA/blob/main/figures/All_Races/Eigenvectors.png)

Explanations of the vectors:

X1 - Positive correlation between `STINT_LAP` and `PERCENT_COMPLETE`, meaning in general, teams choose to lap with the same tire for longer as the race get to an end.

X2 - Positive correlation between `TIME_DELTA` and `GAP_DELTA`, meaning, the slower the driver is relative to the grid, the gap to the leader will be higher.

X3 - Negative correlation between `STINT_LAP` and `GAP_DELTA`, meaning the more laps you go with the same tires, the gap to the leader will drop. (While the performance of the tires degrade, and we would expect the gap to get bigger. The explanation, most likely, is that when the car pits, they rejoin on a lower grid position.)

Plotting the data according to the 3 vectors, while it explains the 90% of the variance, doesn't really show a pattern.

![](https://github.com/eren-ture/F1_Tire_Lap_Analysis_PCA/blob/main/figures/All_Races/pair_plot_afterPCA.png)

So, I tried to get more reasonable results using a single race.

## Analysis of the 2015 Australian GP
### Visualisation

![](https://github.com/eren-ture/F1_Tire_Lap_Analysis_PCA/blob/main/figures/Australia_2015/pair_plot_beforePCA.png)

Notes:
- The normalizations for all the races are not present in this visualization, as we are only analysing one race. (`LAP`, `TIME`, `GAP`)
- The anomaly laps are removed by 4 standard deviations rather than 2.

The same correlations can be seen in this pair graph too.

### PCA

The results of the covariance are what we expect from all race data.

![](https://github.com/eren-ture/F1_Tire_Lap_Analysis_PCA/blob/main/figures/Australia_2015/COV_Matrix.png)

With the single race data, we see that more than 80% of the variance is explained in 2 dimensions.

![](https://github.com/eren-ture/F1_Tire_Lap_Analysis_PCA/blob/main/figures/Australia_2015/eigenvalues.png)

**Eigenvactors**

![](https://github.com/eren-ture/F1_Tire_Lap_Analysis_PCA/blob/main/figures/Australia_2015/eigenvectors_1.png)

X1 - Influenced most by `TIME`, `LAP` and `STINT_LAP`.

This explains the negative relation between the lap time and laps raced/stint laps. In earlier laps the cars are heavier than at the end of the race, because of fuel consumption. This means lighter cars, leading to faster acceleration. Also, the time decrease in later laps can be attributed to “track evolution”, the rubber build-up on the asphalt, making the track itself grippier, leading to faster cornering speeds.

X2 - The positive relation between `GAP` and `TIME`, explaining if the driver is lapping slower than the grid, the gap to the leader will be higher. This can be attributed to driver skill or a slower car.

![](https://github.com/eren-ture/F1_Tire_Lap_Analysis_PCA/blob/main/figures/Australia_2015/plot_final.png)

Now plotting the data-points according to the vectors the tires are better clustered:
- X1: Track Evolution / Car Lightness
- X2: Driver Ranking (Lower is better) / Car Ranking (Lower is better)

## Conclusion

Getting a single tactic / rule for tires on all races proved impossible by PCA.

As for the 2015 Australian GP, we can say:
- As more fuel is consumed during the race, and the track evolved, teams chose to lap with Medium compound tires.
- The driver skill or a faster car didn't affect the choice of tires.
