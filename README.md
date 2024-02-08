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

## Analysis

