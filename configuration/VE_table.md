---
title: Fuel (VE) table / map
description: Configuration of the main fuel / VE table 
published: true
date: 2020-06-25T05:28:43.871Z
tags: fuel, fuel table, tuning, ve table
editor: markdown
---

# Fuel (VE) table

The fuel or VE table is the primary method of controlling the amount of fuel that will be injected at each speed/load point. 

![ve_table.png](/img/tuning/ve_table.png){.align-center width=450}

## Configuration
The fuel map is a 3D, interpolated table that uses RPM and fuel load to lookup the desired VE value. The fuel load axis is determined by whether you are using Speed Density (MAP kPa) or Alpha-N (TPS) for your fuel load (See [Engine_Constants](/en/configuration/Engine_Constants))

The values in this table represent a percentage of the `Required Fuel` amount that will be injected when the engine is at a given speed/load point. 

### Options
- **Multiply VE value by MAP:Baro ratio:** Enabling this option 'flattens' the fuel table by multiplying the value in the current speed/load point by the MAP value divided by the Baro value. You can tune with or without this option enabled, but it is generally recommended to be turned on as it will allow for simpler and more predictable tuning results. 
> **Warning:** Changing this value will require retuning of the fuel map!
{.is-warning}

- **Multiply by ration of AFR to Target AFR:** This option is normally set to `No` for most setups. It allows basic close loop feedback by adjust the base fuel amount according to how far away from the target AFR the engine is currently running. 



## Secondary Fuel table

![2nd_fuel_table.png](/img/tuning/2nd_fuel_table.png){.align-center width=450}

Speeduino also has the ability to use a secondary fuel table which allows for blended and switched mode fueling. There are 2 blended modes and 2 switched modes available.

Blended fuel modes work in conjunction with the primary fuel table to come up with a single, combined VE. Switched fuel modes are where either the primary or secondary fuel table is used, but not both at the same time. Which table is being used at any given time can be configured based on either an external input (Eg dash switch) or set via certain conditions. 


### Multiplied %
This is a blended fuel mode (ie it uses both the primary and secondary fuel tables together) that allows for different load and RPM axis to be combined. Commonly this is used for having primary and secondary fuel tables with different load sources (**Eg:** Primary map using TPS and secondary map using manifold pressure). 

This mode is often used on engines with Individual Throttle Bodies (ITBs) to allow TPS and MAP based tables to be combined.

The final fuel value is derived from treating both values (Primary and Secondary) as percentages and multiplying them together. 
#### Example 1
**Primary Fuel table value:** 75
**Secondary fuel table value:** 100
**Final value:** 75 

#### Example 2
**Primary Fuel table value:** 80
**Secondary fuel table value:** 150
**Final value:** 120

#### Example 3
**Primary Fuel table value:** 90
**Secondary fuel table value:** 80
**Final value:** 72

### Added
This is a blended fuel mode that is very similar to the above `Multipled %` mode. The only difference between the two is that instead of multiplying the values from the primary and secondary tables, the 2 are added together. 

This is a less commonly used mode, but is an alternative in the same setups that you would use `Multiplied %`

### Switched - Conditional
Conditional switched mode will allow use of the 2nd fuel table when a certain value goes above a defined level. The available switching values are:

* RPM
* Ethanol content
* MAP 
* TPS

Dpending on the desired outcome, this can be used to expand the resolution of the main fuel table, automatically handle alternate fuels or as an alternative ITB mode (Particularly if running boosted ITBs). 

### Switched - Input based
