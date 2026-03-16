# Results

## Experiment 1

During the exploratory part of the project, two experiments were conducted. In the first experiment, the forage-step parameter was kept constant (at the value of 1), while the value of migration-step varied. The experiment was conducted with 5 different values for the migration-step parameter. There were 120 runs per parameter (600 runs in total). The stop conditions of each run were the death of all agents, or that the run reaches 20000 ticks. The experiment recorded the duration of each simulation run. 

  Migration Step | Mean Duration of Runs (in ticks) |
 |----------------|----------------------------------|
 | 10             | 11680.89                         |
 | 15             | 14901.19                         |
 | 20             | 17203.73                         |
 | 25             | 16337.18                         |
 | 30             | 16238.74                         |

<img src="https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/9ea4fe4308296f4da78060e9ee6dd03fcf990e8a/img/experiment%201.png" alt="Alt Text" width="70%" height="70%">

## Experiment 2

It was then decided that two migration-step values from the previous experiment will be used for further analysis. Those were 15 and 25, since they proved to be the most successful. However, in the next experiment, the value of forage-step was changed too. The experiment was conducted with three different values of forage-step step parameter (1, 2 and 3). Each combination of variables was run one hundred times, with 600 runs in total. 

  Migration Step | Forage Step | Mean Duration of Runs (in ticks) |
 |----------------|-------------|----------------------------------|
 | 15             | 1           | 15158.79                         |
 | 15             | 2           | 7184.43                          |
 | 15             | 3           | 2208.85                          |
 | 25             | 1           | 16676.29                         |
 | 25             | 2           | 4662.84                          |
 | 25             | 3           | 897.33                           |

<img src="https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/9ea4fe4308296f4da78060e9ee6dd03fcf990e8a/img/experiment%202.png" alt="Alt Text" width="70%" height="70%">

## Discussion

The results of experiment 1 showed that migration-step parameter influences the success rate of the foraging strategy. Out of the five values with which the experiment was run, the lowest one (10) rendered the least successful foraging strategy. The foragers were most successful when the forage-step parameter was set to 20. However, parameter values higher than that (25, 30) showed less success.
Experiment 2 showed that forage-step also influences success of foragers. With the increase in the value of forage-step, the success rate of the foraging strategy decreases. 
The premise of this paper was to observe  difference between foraging behaviour with a higher value of migration-step and forage-step (SB) and a foraging behaviour with lower value of those two parameters (HP). However, this is not possible from the work done so far. As can be observed from experiment 2, the increase in forage-step leads to sudden decrease in the success of a foraging strategy, regardless of the value of migration-step. This is at least the case for the given parameter values. 
On the other hand, experiment 1 showed that, given that the environment stays the same, change in migration-step influences the success of the foraging strategy. However, it is questionable whether this result sheds any light to the initial archaeological question. The increase in forager success rates with the increase of migration-step in experiment 1 seems to be easily explained: a if the migration-step is too short, the camp is not able to escape the area with depleted resources. The success rate then falls for migration-step values higher than 20 because foragers start expending too much energy on migration than is necessary to escape from the areas where they depleted the resources. 
