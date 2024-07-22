# MinecraftDynamics
Some equation governing Minecraft physics engine and the way to get to it

# Introduction
Minecraft physics engine update physics properties every ticks, the goal of this document is to understand how the physics engine works with various Minecraft objects and develop equation to predict the trajectory of entities. We will use most common units used in Minecraft to describe the dynamic of objects.  
The distance unit will be the meter, where 1 meter is the width of a block.  
The time unit will be the tick, where 1 tick = 1/20 seconds.  
Because the time is discrete we will avoid Newtonian mechanic equations which usually require continuous time. Using these equation will lead to huge inaccuracy.

# Acceleration and velocity
We will first need to understand what velocity and acceleration are. Basically velocity is the rate of change of position and acceleration is the rate of change of velocity, which can be translated into equations that you may have already encountered, let's split it into the equation used with discrete and continuous time :

|            |Continuous                 |Discrete                                       |
|------------|---------------------------|-----------------------------------------------|
|Velocity    |$v(t) = \frac{d x(t)}{d t}$|$v_t = \frac{ x_{t+\Delta t} - x_t }{\Delta t}$|
|Acceleration|$a(t) = \frac{d v(t)}{d t}$|$a_t = \frac{ v_{t+\Delta t} - v_t }{\Delta t}$|

In Minecraft physics engine, the standard timestamp between updates is 1 tick, so $\Delta t = 1$, we can simplify expression like this :

$$ v_t = \frac{ x_{t+1} - x_t }{1} = x_{t+1} - x_t $$  
$$ a_t = \frac{ v_{t+1} - v_t }{1} = v_{t+1} - v_t $$

# Forces
Let's talk about forces and illustrate how they work with Newtonian mechanic equation (*even if they won't work to predict game physic, they are useful to understand what forces are*). The Newton's second law of motion tells us that forces applied on an object are proportional to acceleration of this object (the proportionality constant being mass) :  

```math
\sum{ \vec{F} } = m \vec{a}
```
