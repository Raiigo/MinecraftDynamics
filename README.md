# MinecraftDynamics
Some equation governing Minecraft physics engine and the way to get to it

# Introduction
Minecraft physics engine update physics properties every ticks, the goal of this document is to understand how the physics engine works with various Minecraft objects and develop equation to predict the trajectory of entities. We will use most common units used in Minecraft to describe the dynamic of objects.
The distance unit will be the meter, where 1 meter is the width of a block.  
The time unit will be the tick, where 1 tick = 1/20 seconds.  
Because the time is discrete we will avoid Newtonian mechanic equations which usually require continuous time. Using these equation will lead to huge inaccuracy.

# Acceleration and velocity
First, let's talk about forces and illustrate how they work with Newtonian mechanic equation (*even if they won't work to predict game physic, they are useful to understand what forces are*). The Newton's second law of motion tells us that forces applied on an object are proportional to acceleration of this object (the proportionality constant being mass) :

```math
\sum F = m a
```
