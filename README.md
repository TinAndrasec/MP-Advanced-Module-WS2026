# MP-Advanced-Module-WS2026
This repository contains documentation for an archaeological agent-based model developed for an exam in the course "Agent-based modelling for archaeologists" taken at the University of Cologne.

## Procedures

### _Setup_
Before the start of a simulation run, the “setup” procedure (figure 1) has to be run. To start with, all variables are cleared using the “clear-all” procedure. Next, the generation of the simulation landscape begins. Patches are set as the observer, and each patch runs an if-else statement in which the value of a randomly generated number is compared to the previously defined “resource-density” parameter. If the randomly generated number is lower than the “resource-density”, energy is allocated to a patch: its “penergy” variable is set to the value of “max-penergy” parameter. Furthermore, “pempty” is set to “false”. This variable will later be used to evaluate whether the energy at the patch should be replenished. If that is not the case, “penergy” is set to 0, and “pempty” is set to “true”. Afterwards, colour of the patch is set in such a way to reflect the amount of energy available on the patch. 

Secondly, the procedure creates agents belonging to the breed “camps”. The number of camps which are thus created is specified by the “camp-number” parametre. The camps are then randomly distributed across the simulation landscape, and their starting population and stored energy are set up according to the value of “starting-population” and “init-camp-energy” parameters. 

Then, for every 10 units of population, a camp hatches a forager. The foragers then set their “my-home” variable to include the camp which hatched them, after which they set their “return-home” and “migrate-now” variables to “false”. The starting encounter list is setup: it contains 10 values, each equalling twice the amount of “max-penergy”. “Encounter-rate” is set to the mean of the “encounter-list”. Consequently, “forager-energy” is set to the value of “init-forager-energy”. 

Finally, the observer again switches to the camps, and each camp stores their newly created foragers in their my-foragers agentset variable. Then, the tick counter is reset. 

![image alt](https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/49c447afa66fb33ce0dd4dfe64f7f6e188d73b23/img/setup_flowchart.png)

### _Go_

![image alt](https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/521faf935bd5041a089da672c9f2d3d5520922ff/img/go_flowchart.png)

### _Forage_

![image alt](https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/641403b6d481a18b19f4733a0beb12dc2917c1e9/img/forage_flowchart.png)

### _Gather_

![image alt](https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/521faf935bd5041a089da672c9f2d3d5520922ff/img/gather_flowchart.png)

### _Migrate-check_
![image alt](https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/521faf935bd5041a089da672c9f2d3d5520922ff/img/migrate-check_flowchart.png)
### _Split-population_

![image alt](https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/521faf935bd5041a089da672c9f2d3d5520922ff/img/split-population_flowchart.png)

### _Migrate_

![image alt](https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/521faf935bd5041a089da672c9f2d3d5520922ff/img/migrate_flowchart.png)

### _Camps-eat_

### _Foragers-eat_

### _Travel-home_

![image alt](https://github.com/TinAndrasec/MP-Advanced-Module-WS2026/blob/521faf935bd5041a089da672c9f2d3d5520922ff/img/travel-home_flowchart.png)

### _Transfer-storage_

### _Increase-population-check_

### _Update-display_

_Regrow-patches_

