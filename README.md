# MP-Advanced-Module-WS2026
This repository contains documentation for an archaeological agent-based model developed for an exam in the course "Agent-based modelling for archaeologists" taken at the University of Cologne.

## Purpose
The purpose of this model is to simulate the behaviour of Palaeolithic hunter-gatherer communities. The model creates a randomly generated landscape, with energy being randomly distributed across the landscape. Hunter-gatherer communities are represented by agents of breed “camps”. Each camp creates foraging parties, represented by the breed “forager”, which harvests energy from the environment and brings it to the camp. After the energy in the vicinity of the camp has been depleted, the camps relocate to another part of the landscape. As time progresses, camps and forager parties consume the gathered energy. In the case that all the energy reserves of a camp are exhausted, the camp perishes. If the camps are successful in harvesting energy, the population grows, and eventually the camps split, creating new communities. The model thus enables comparison of different foraging strategies.

## Parameters and variables

The model was created in NetLogo development environment version 7.0.3. It requires the following input parameters: 

  Parameter                 | Type                  | Value Range  | Description                                                                                             |
 |---------------------------|-----------------------|--------------|---------------------------------------------------------------------------------------------------------|
 | camp-number               | Integer               | 1 to 10      | Number of camps at the start of the simulation run.                                                     |
 | starting-population       | Integer               | 10 to 100    | Population of starting camps.                                                                           |
 | init-camp-energy          | Integer               | 0 to 2000    | Amount of energy stored by each starting camp.                                                          |
 | max-penergy               | Integer               | 0 to 100     | Maximum value of the penergy variable of a patch.                                                       |
 | init-forager-energy       | Integer               | 0 to 100     | Amount of energy stored by each starting forager.                                                       |
 | resource-density          | Integer               | 0 to 100     | Density of energy in the environment. The chance that in a simulation run a patch will contain energy.  |
 | gather-rate               | Integer               | 0 to 100     | Amount of energy which a forager will harvest in a tick from a patch where it is located.               |
 | growth-rate               | Floating point number | 0 to 0.25    | Amount by which the energy in harvested patches recovers each tick.                                     |
 | pop-growth-chance         | Floating point number | 0 to 0.1     | Chance per tick that, given there is enough energy, the population of a camp will grow.                 |
 | forage-step               | Integer               | 1 to 5       | Distance which a forager covers each tick.                                                              |
 | forager-movement-cost     | Integer               | 1 to 10      | Amount of energy consumed by a forager when traversing a length of an unit of distance while foraging.  |
 | camp-movement-cost        | Integer               | 1 to 100     | The amount of energy consumed by a camp when traversing a length of an unit of distance while migrating.|
 | return-home-threshold     | Integer               | 1 to 100     | After harvesting this amount of energy from the environment, the forager returns back to its camp.      |
 | migration-step            | Integer               | 1 to 50      | Distance a camp covers during a single migration event.                                                 |
 | pop-energy-consumption    | Integer               | 1 to 10      | Amount of energy a unit of population consumes each tick.                                               |
 | split-pop-pop-threshold   | Integer               | 0 to 100     | Population value at which, once reached, a camp will split into two camps.                              |
 | split-pop-energy-threshold| Integer               | 0 to 4000    | Stored energy required for splitting the population into two camps.                                     |

 <img src="https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/91075727c9f66e08176e5531fb6ac0d4fa28b61f/img/Screenshot_GUI.png" alt="Alt Text" width="130%" height="130%">

 The model has three agent (or, using NetLogo terminology, “turtle”) breeds: patches, camps, foragers, each breed possessing the following variables:

   Breed    | Variable       | Type                  | Description                                                                                  |
 |:---------|:---------------|:----------------------|:---------------------------------------------------------------------------------------------|
 | patches  | penergy        | Floating point number | The amount of energy available for harvest on a patch.                                       |
 | patches  | pempty         | Boolean               | Indicates whether energy can be harvested from a patch.                                      |
 | camps    | my-foragers    | Agentset              | Agentset containing all foragers belonging to a camp.                                        |
 | camps    | camp-energy    | Floating point number | Amount of energy stored in a camp.                                                           |
 | camps    | migrate-now    | Boolean               | Indicates whether the migration process should begin.                                        |
 | camps    | population     | Integer               | Population of a camp.                                                                        |
 | foragers | my-home        | Agent                 | Camp to which an agent belongs.                                                              |
 | foragers | encounter-list | List                  | Energy values harvested from the last 10 visited patches.                                    |
 | foragers | encounter-rate | Floating point number | Average value of the elements in the encounter-list.                                         |
 | foragers | forager-energy | Integer               | Energy stored by each forager.                                                               |
 | foragers | return-home    | Boolean               | Indicates whether a forager starts returning to its camp (home), ending the foraging process. |
 | foragers | migrate-now    | Boolean               | Indicates whether the camp to which a forager belongs will begin its migration process.      |

 The simulation landscape is represented by the NetLogo world, with maximum x coordinate of the patches being 50, and maximum y coordinate set at 40. The world wraps both horizontally and vertically. 

  <img src="https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/293b6bded0fe7133f85a70b2e638aabb41ef3ff4/img/Screenshot_sim_run.png" alt="Alt Text" width="130%" height="130%">


 


## Procedures

The model contains the following procedures:

  Procedures            | Observer     |
 |-----------------------|--------------|
 | setup                 | global       |
 | go                    | global       |
 | forage                | foragers     |
 | gather                | foragers     |
 | migrate-check         | camps        |
 | split-population      | camps        |
 | migrate               | camps        |
 | camps-eat             | camps        |
 | foragers-eat          | foragers     |
 | Transfer-storage      | foragers     |
 | increase-population-check | camps    |
 | update-display        | patches      |
 | regrow-patches        | patches      |

 
### _Setup_

Before the start of a simulation run, the “setup” procedure (figure 1) has to be run. To start with, all variables are cleared using the “clear-all” procedure. Next, the generation of the simulation landscape begins. Patches are set as the observer, and each patch runs an if-else statement in which the value of a randomly generated number is compared to the previously defined “resource-density” parameter. If the randomly generated number is lower than the “resource-density”, energy is allocated to a patch: its “penergy” variable is set to the value of “max-penergy” parameter. Furthermore, “pempty” is set to “false”. This variable will later be used to evaluate whether the energy at the patch should be replenished. If that is not the case, “penergy” is set to 0, and “pempty” is set to “true”. Afterwards, colour of the patch is set in such a way to reflect the amount of energy available on the patch. 

Secondly, the procedure creates agents belonging to the breed “camps”. The number of camps which are thus created is specified by the “camp-number” parametre. The camps are then randomly distributed across the simulation landscape, and their starting population and stored energy are set up according to the value of “starting-population” and “init-camp-energy” parameters. 

Then, for every 10 units of population, a camp hatches a forager. The foragers then set their “my-home” variable to include the camp which hatched them, after which they set their “return-home” and “migrate-now” variables to “false”. The starting encounter list is setup: it contains 10 values, each equalling twice the amount of “max-penergy”. “Encounter-rate” is set to the mean of the “encounter-list”. Consequently, “forager-energy” is set to the value of “init-forager-energy”. 

Finally, the observer again switches to the camps, and each camp stores their newly created foragers in their my-foragers agentset variable. Then, the tick counter is reset. 

<img src="https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/49c447afa66fb33ce0dd4dfe64f7f6e188d73b23/img/setup_flowchart.png" alt="Alt Text" width="50%" height="50%">


### _Go_

At the start of the “go” procedure, foragers are set as the observer. First, a forager runs the “foragers-eat”, procedure, thus reducing its “forager-energy” variable by the set amount. Afterwards, a forager is asked to check the value of its “return-home” and “migrate-now” variables. If either one of those variables is set to “true”, the forager runs the “travel-home” procedure, heading to its camp. Otherwise, the “forage” procedure is run, with the agent traversing the environment and harvesting the energy, if possible. This is repeated for all agents of the breed.

Next, camps are set as the observer. First, a camp runs the “camps-eat” procedure, thus expending some energy as defined by the input parameters. After that, the camp runs the “migrate-check” procedure, evaluating whether the conditions for migration are met. Following that, the camp runs the “increase-population-check” procedure, ascertaining whether the population of the camp should be increased. This process is repeated for every instance of a camp. 

Subsequently, patches are set as the observer. Initially, a patch runs the “regrow-patches” procedure, which increases the amount of energy in the patches which contain resources. Afterwards, the “update-display” procedure is run, changing the colour of the patch according to the new value of its “penergy” value. This is again repeated for all patches.
Finally, the tick number is advanced by one. 

<img src="https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/521faf935bd5041a089da672c9f2d3d5520922ff/img/go_flowchart.png" alt="Alt Text" width="50%" height="50%">

### _Forage_

The “forage” procedure (figure 3) is run with foragers are set as the observer. Initially, a forager is rotated by a random number of degrees. Next, it moves forward, the distance of this movement being set by the “forage-step” parameter. Thereafter, an amount of energy (as set by the starting parameters) is reduced from its energy storage. Next, it runs the gather procedure. Finally, it evaluates whether the value of its stored energy is higher than the “return-home-threshold” parameter. This being the case, it sets the value of the return-home variable to “true”.

<img src="https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/641403b6d481a18b19f4733a0beb12dc2917c1e9/img/forage_flowchart.png" alt="Alt Text" width="65%" height="65%">

### _Gather_

The “gather” procedure is run with foragers set as the observer. In the code, it is only called within the “forage” procedure. To start with, two temporary variables are initiated: “patch-energy” and “gather-amount”. “Patch-energy” is set to the value of “penergy” variable of the patch where a forager is currently located. In the case that the value of “patch-energy” is greater than zero, a process where value is added to “gather-amount” is activated. Otherwise, the value of zero is added to the forager’s “encounter-list”, placing it as the first item of the list. Next, the last item of the list is removed.

In the case that the value of “patch-energy” is greater than zero, another conditional statement is run. This time, the “patch-energy” variable is compared to the “gather-rate”. If “gather-rate” is higher than “patch-energy”, “gather-amount” is set to the value of “gather-rate”. In other words, the forager does not gather all the available energy from the patch, but only the maximum amount he is allowed to gather by the initial parameters of the simulation run. Next, the patch where the forager is located (“patch-here”) is set as the observer, and its “penergy” variable is decreased by the “gather-amount”. 

n the event that “patch-energy” is less than “gather-rate”, the “gather-amount” is set to the value of “patch-energy.” In simple terms, the forager collects all the energy which is available on the patch. Again, the “patch-here” is set as the observer, and its “penergy” is then set to zero. 
After energy has been gathered, the “forager-energy” variable of the forager is increased by the gathered amount. Next, the forager’s “encounter-list” is updated, as was described above.

<img src="https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/521faf935bd5041a089da672c9f2d3d5520922ff/img/gather_flowchart.png" alt="Alt Text" width="65%" height="65%">

### _Migrate-check_

The “migrate-check” procedure is run with camps as the observer. To start with, it evaluates whether the “migrate-now” variable of a camp is set to “true”. In the case that it is set to “true”, the camp will evaluate whether all the conditions for migration are satisfied.

Thereafter, a camp will check whether there are any agents in its “my-foragers” variable, i. e. it will check whether any of its foragers are still alive. In the case that there are no living foragers which belong to that camp, the “migrate procedure” will be run. However, before that, it will be evaluated whether the values of both population and stored energy exceed the threshold for splitting the camp into two. If that is the case, the “split-population” procedure will be run before the “migrate” procedure. 

<img src="https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/521faf935bd5041a089da672c9f2d3d5520922ff/img/migrate-check_flowchart.png" alt="Alt Text" width="65%" height="65%">

### _Split-population_

The “split-population” procedure is run within the “migrate-check” procedure. Therefore, a camp is an observer. The camp is asked to hatch another camp. By the definition of the “hatch-<breed>” function in NetLogo programming language, hatched agents inherit all variables from the observer agent. Next, the newly hatched camp sets the values of its population and energy variables to half of that of the original camp. After that, the original camp also halves its population and stored energy.

<img src="https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/521faf935bd5041a089da672c9f2d3d5520922ff/img/split-population_flowchart.png" alt="Alt Text" width="50%" height="50%">

### _Migrate_

The “migrate” procedure is run within the “migrate-check” procedure, with camps being the observer. Firstly, the camp running the procedure rotates for a random number of degrees, and makes a step forward. The length of the step depends on the value of the “migration-step” parameter. Thereafter, a variable named “done” is initialised, which is used to control a while loop. The purpose of the loop is to ensure that the migrating camp migrates to an empty patch. If “done” equals “false”, the camp will evaluate whether there are any other camps present at its patch. If there is another camp present, the camp will move to one of the neighbouring patches. Otherwise, the value of “done” will be set to true, thus ending the while loop. 

Following that, an amount of energy will be removed from the energy storage of the camp. This amount will depend on the length of the migration step, and the “camp-movement-cost” parameter, set at the beginning of the simulation run. Furthermore, the “migrate-now” variable of the camp will be set to “false”. Finally, depending on the value of its population variable, the camp will hatch a number of foragers, which will be accessible through its “my-foragers” agentset variable.

<img src="https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/521faf935bd5041a089da672c9f2d3d5520922ff/img/migrate_flowchart.png" alt="Alt Text" width="70%" height="70%">


### _Camps-eat_

This procedure is called within the “go” procedure, with camps being the observer. It reduces the value of a camp’s “camp-energy” variable by the multiple of “pop-energy-consumption” and the population of a camp. In other words, “pop-energy-consumption” parameter regulates how much energy an unit of population will consume in a tick. After that, the value of the camp’s “camp-energy” variable is checked: if it is less or equal to zero, both the camp and it’s foragers are asked to die.

### _Foragers-eat_

This procedure is called within the “go” procedure, with foragers being the observer. It checks whether the “forager-energy” variable of a forager is less or equal to zero. If that is the case, the forager will die. 

### _Travel-home_

The “travel-home” procedure is called as part of the “go” procedure, with foragers being set as the observer. After the procedure is called, a forager faces its home camp. Next, an if statement evaluates the proximity of the forager to its home camp: if the forager is located closer to the camp than the length of its foraging step, it moves to the camp’s location. Furthermore, the forager’s stored energy is transferred to the camp, and the “return-home” variable is set to “false”. In the case that the camp started its migration process, indicated by the value of the “migrate-now” variable equalling “true”, the forager dies. 

In the case that the distance from the forager to the camp is greater than the value of the forage step, the forager moves towards its camp, the length of the step being set by the “forage-step” parameter. Finally, the energy cost of this step is subtracted from the forager’s stored energy. 

<img src="https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/521faf935bd5041a089da672c9f2d3d5520922ff/img/travel-home_flowchart.png" alt="Alt Text" width="70%" height="70%">

### _Transfer-storage_

This procedure is called by “travel-home” procedure. It is run when a forager returns to its home camp. In the procedure, the forager transfers most of its stored energy to its home camp, keeping only the amount indicated by “init-forager-energy” parameter. This is done to ensure the forager has enough energy to embark on another foraging expedition. 

### _Increase-population-check_

“Increase-population-check” procedure is run as a part of the “go” procedure, with camps set as the observer. It evaluates whether the amount of stored energy in a camp surpasses the “population-growth-cost”, and if a randomly generated floating point number is smaller than the “population-growth-chance”. If that is the case the camp decreases its “camp-energy” by “population-growth-cost”, and the value of the population variable of the camp is increased by one.

### _Update-display_

Called at the end of the “go” procedure. Patches are set as the observer, and the procedure changes colour of each patch reflect the amount of available energy. 

### _Regrow-patches_

Called at the end of the “go” procedure. Patches are set as the observer. In the case that the “pempty” variable of the patch is set to “false”, and the patch has less energy than the maximum possible energy value, the “penergy” variable of the patch is increased by “growth-rate”. 
