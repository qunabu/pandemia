---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
title: Configuration
permalink: /config/

---

## Simulations 

The [simulation](https://qunabu.github.io/covid-simulation/) is quite straight forward and it is being rendered by the following variables

### Variables 

#### Default variables

* `amount` number of balls on the stage 
* `width` width of the stage in pixels
* `height` height of the stage in pixels
* `cyclesInterval` number of milliseconds new cycle is triggered for example value 100 means that each cycle is trigger for every 100ms

#### Age structure

* `distrAge9` Percentage of age 0-9 distribution from whole population
* `distrAge19` Percentage of age 10-19 distribution from whole population
* `distrAge29` Percentage of age 20-29 distribution from whole population
* `distrAge39` Percentage of age 30-39 distribution from whole population
* `distrAge49` Percentage of age 40-49 distribution from whole population
* `distrAge59` Percentage of age 50-59 distribution from whole population
* `distrAge69` Percentage of age 60-69 distribution from whole population
* `distrAge79` Percentage of age 70-79 distribution from whole population
* `distrAge100` Percentage of age 80-100 distribution from whole population

Value `0.03` for `distrAge9` means that from the whole population 3% has age between 0 and 9 (including 9) and so on. The reason to have age in the simulation is that fatality ratio is different for each range. All the percentage is sum must equals 1 (which is 100%).

#### Fatality ratio

* `fatalAge9` Percentage chance of fatal sickness of age 0-9 distribution from whole population
* `fatalAge19` Percentage chance of fatal sickness of age 10-19 distribution from whole population
* `fatalAge29` Percentage chance of fatal sickness of age 20-29 distribution from whole population
* `fatalAge39` Percentage chance of fatal sickness of age 30-39 distribution from whole population
* `fatalAge49` Percentage chance of fatal sickness of age 40-49 distribution from whole population
* `fatalAge59` Percentage chance of fatal sickness of age 50-59 distribution from whole population
* `fatalAge69` Percentage chance of fatal sickness of age 60-69 distribution from whole population
* `fatalAge79` Percentage chance of fatal sickness of age 70-79 distribution from whole population
* `fatalAge100` Percentage chance of fatal sickness of age 80-100 distribution from whole population

Value `0.002%` for `fatalAge9` means that person with age in that range has 0.002% change of fatal sickness, person aged more the 80 has 9.3% change of fatal sickness, etc. 

#### Infection variables

* `percInfected` Percentage of initially infected, example: 0.03 means that for 100 balls 3 will be infected at the beginning of the simulation 
* `probInfection` Probability that one ball will infect another in case of collision
* `probInfectionSick` Probability that infection will convert to sickness
* `cyclesToRecoverOrDie` number of cycles to recover or fatal sickness
* `cyclesInterval` time of each cycle in ms

#### Level of hygiene

* `casual` = `1.5`  Casual - No one washing their hands
* `normal` = `1` Normal - 67,3% of people washing hands
* `brutal` = `0.8` Brutal - Compulsive hand washing
 
#### Quarantine
 
* `quarantineWalls` number of walls number of walls that stage will be divided by. 
* `quarantineNotMove` Percentage of balls that are not moving, Number of balls that will not move during the whole simulation. Example: 0.03 means that for 100 balls 3 will not move. 
* `quarantineWallOpen` Does wall do open after some time decide if the wall is solid or will collapse in time 
* `quarantineCycWallOpen` Cycles after walls starts to open
* `quarantineCycMove` Cycles after ball start moving again

#### Hospitalization 

* `hospLvl` Maximal level of hospitalization for population
* `noHospMultiply` Multiplier of fatal sickness of sicks person that doesn't get hospitalization
* `hospiAge9` Age 0-9 % requiring critical hospitalization
* `hospiAge19` Age 10-19 % requiring critical hospitalization
* `hospiAge29` Age 20-29 % requiring critical hospitalization
* `hospiAge39` Age 30-39 % requiring critical hospitalization
* `hospiAge49` Age 40-49 % requiring critical hospitalization
* `hospiAge59` Age 50-59 % requiring critical hospitalization
* `hospiAge69` Age 60-69 % requiring critical hospitalization
* `hospiAge79` Age 70-79 % requiring critical hospitalization
* `hospiAge10` Age 80-100 % requiring critical hospitalization

### Calculations 

`Math.random()` returns random value from range 0 to 1.

Most calculations are based on the `cycle`, every number of milliseconds new cycle is triggered, this is controlled that `cyclesInterval` variable, example: value 100 means that each cycle is trigger for every 100ms

#### Infection

If `sick` or `infected` ball confronts (ball collide with another) the `healthy` one the change of being infected is calculated with the formula 

```
if (Math.random() < probInfection * hygieneLevel) {
   state = STATES.infected;
}
```

#### Infected to get sick

Once ball is `infected` it starts its own cycle counter, each cycle it increments its value by one. The formula checks if both conditions are true 
1. ball counter is bigger than `cyclesToRecoverOrDie` and `Math.random()` is higher than `0.8`
2. `Math.random()` is lower than `probInfectionSick` 
then ball is `sick` from now 

```
if ( tick++ > cyclesToRecoverOrDie && Math.random() > 0.8 ) {
  if (Math.random() <= probInfectionSick) {
    state = STATES.sick;
  }    
}
```

#### Sick to die or recover 

Once ball is `sick` it starts its own cycle counter (back to 0), each cycle it increments its value by one. The formula checks if both conditiona are true. `probFatality` is taken from `fatalAgeX` based on the `age` of the ball. 
1. ball counter is bigger than `cyclesToRecoverOrDie` and `Math.random()` is higher than `0.8`
2. `Math.random()` is lower than `probFatality` then ball change state to `dead` othewise to `recovered`
then ball is `sick` from now 
3. If the ball is not hospitalised `probFatality` is multiplied by `1 + reqHospi * noHospMultiply` where is taken from config `hospiAgeX` based on the `age` of the ball. 


```
if ( tick++ > cyclesToRecoverOrDie && Math.random() > 0.8) {
 state = Math.random() < probFatality * (isHospialised ? 1 : 1 + reqHospi * noHospMultiply)  ? STATES.dead : STATES.recovered;
}    
```

### Start (and restart) 

Every time simulation starts it draws from the configuration variables.
