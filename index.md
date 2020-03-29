---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
title: Hello Infected World 
---

# Introduction 

bla bla bla

## Simulations 

The simulation is quite straight forward and it is being rendered by the following variables 

### Variables 

*Default variables* 

* `amount` number of balls on the stage 
* `width` width of the stage in pixels
* `height` height of the stage in pixels

*Age structure* 

* `distrAge9` "Percentage of age 0-9 distribition from whole population"
* `distrAge19` "Percentage of age 10-19 distribition from whole population"
* `distrAge29` "Percentage of age 20-29 distribition from whole population"
* `distrAge39` "Percentage of age 30-39 distribition from whole population"
* `distrAge49` "Percentage of age 40-49 distribition from whole population"
* `distrAge59` "Percentage of age 50-59 distribition from whole population"
* `distrAge69` "Percentage of age 60-69 distribition from whole population"
* `distrAge79` "Percentage of age 70-79 distribition from whole population"
* `distrAge100` "Percentage of age 80-100 distribition from whole population"

0-9 3% means that from the whole population 3% has age between 0 to 9 (including 9) and so on. The reason to have age in the simulatio is that fatality ratio is different for each range. All the percenage is sum must eqals 100% 

*Fatality ratio*

* `fatalAge9` "Percentage chance of fatal sickness of age 0-9 distribition from whole population"
* `fatalAge19` "Percentage chance of fatal sickness of age 10-19 distribition from whole population"
* `fatalAge29` "Percentage chance of fatal sickness of age 20-29 distribition from whole population"
* `fatalAge39` "Percentage chance of fatal sickness of age 30-39 distribition from whole population"
* `fatalAge49` "Percentage chance of fatal sickness of age 40-49 distribition from whole population"
* `fatalAge59` "Percentage chance of fatal sickness of age 50-59 distribition from whole population"
* `fatalAge69` "Percentage chance of fatal sickness of age 60-69 distribition from whole population"
* `fatalAge79` "Percentage chance of fatal sickness of age 70-79 distribition from whole population"
* `fatalAge100` "Percentage chance of fatal sickness of age 80-100 distribition from whole population"

0-9 3% 0.002% means that person with age in that range has 0.002% change of fatal sickenes, person aged more the 80 has 9.3% change of fatal sickenes.

*Infection variables*
* `percInfected` "Percentage of initialy infected", eg 0.03 means that for 100 balls 3 will be infected at the beginning of the simulation 
* `probInfection` "Propability that one ball will infect another in case of collision"
* `probInfectionSick` "Propability that infection will convert to sickness"
* `cyclesToRecoverOrDie` "number of cycles to recover or fatal sickness"
* `cyclesInterval` "time of each cycle in ms"

*Level of hygiene*

* `casual` = `1.5`  "Casual - No one washing their hands"
* `normal` = `1` "Normal - 67,3% of pepole washing hands"
* `brutal` = `0.8` "Brutal - Compulsive hand washing"
 
 *Quarantine*
 
* `quarantineWalls` "number of walls" number of walls that stage will be divided by. 
* `quarantineNotMove` "Percentage of balls that are not moving", Number of balls that will not move during the whole simulation. eg 0.03 means that from 100 balls 3 will not move. 
* `quarantineWallOpen` "Does wall do open after some time" decide if the wall is solid or will collapse in time 

### Calculations 

`Math.random()` returns random value from range 0 to 1.

Most calculations are based on the `cycle`, every numer of miliseconds new cycle is triggered, this is controlled that `cyclesInterval` variable, eg value 100 means that each cycle is trigger for every 100ms

*Infection*

If `sick` or `infected` ball confronts (ball collide with another) the `healthy` one the change of being infected is caluclated with the formula 

```
if (Math.random() < probInfection * hygieneLevel) {
   state = STATES.infected;
}
```

*Infected to get sick* 

Once ball is `infected` it starts its own cycle counter, each cycle it increments its value by one. The formula checks if both conditiona are true 
1. ball counter is bigger then `cyclesToRecoverOrDie` and `Math.random()` is higher then `0.8`
2. `Math.random()` is lower then `probInfectionSick` 
then ball is `sick` from now 

```
if ( tick++ > cyclesToRecoverOrDie && Math.random() > 0.8 ) {
  if (Math.random() <= probInfectionSick) {
    state = STATES.sick;
  }    
}
```

*Sick to die or recover 




#### Simple

Simple simulation. 300 people, propability of infection is 100% same is propability of that infection will convert to sickness. Fatality ratio is 2% regardless of age. 

{% raw %}
<iframe src="https://qunabu.github.io/covid-simulation/?width=1024&height=509&amount=300&probInfection=1&probInfectionSick=1&fatalAge9=0.002&fatalAge19=0.002&fatalAge29=0.002&fatalAge39=0.002&fatalAge49=0.002&fatalAge59=0.002&fatalAge69=0.002&fatalAge79=0.002&fatalAge100=0.002&quarantineWalls=0&quarantineNotMove=0&iframe=1" width="1050" height="649"></iframe>
{% endraw %}





#### Age structure 

This simulation has a different fatality ratio based on the age. Age structure is normalized 

| Age      | 0-9 | 10-19 | 20-29 | 30-39 | 40-49 | 50-59 | 60-69 | 70-79 | 80+ |
|----------|-----|-------|-------|-------|-------|-------|-------|-------|-----|
| Stucture | 3%  | 7%    | 14%   | 19%   | 20%   | 19%   | 13%   | 3%    | 2%  |

0-9 3% means that from the whole population 3% has age between 0 to 9 (including 9) and so on. The reason to have age in the simulatio is that fatality ratio is different for each range. All the percenage is sum must eqals 100% 

| Age      | 0-9    | 10-19  | 20-29 | 30-39 | 40-49 | 50-59 | 60-69 | 70-79 | 80+  |
|----------|--------|--------|-------|-------|-------|-------|-------|-------|------|
| Stucture | 0.002% | 0.006% | 0.03% | 0.08% | 0.15% | 0.6%  | 2.2%  | 5.1%  | 9.3% |

0-9 3% 0.002% means that person with age in that range has 0.002% change of fatal sickenes, person aged more the 80 has 9.3% change of fatal sickenes.

{% raw %}
<iframe src="https://qunabu.github.io/covid-simulation/?width=1024&height=509&amount=289&quarantineWalls=0&quarantineNotMove=0&iframe=1" width="1050" height="649"></iframe>
{% endraw %}




