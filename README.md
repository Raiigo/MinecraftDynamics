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

$$v_t = \frac{ x_{t+1} - x_t }{1} = x_{t+1} - x_t$$  

$$a_t = \frac{ v_{t+1} - v_t }{1} = v_{t+1} - v_t$$  

These are the two expression we will be using for our equations later on.

# Forces
Let's talk about forces and illustrate how they work with Newtonian mechanic equation (*even if they won't work to predict game physic, they are useful to understand what forces are*). The Newton's second law of motion tells us that forces applied on an object are proportional to acceleration of this object (the proportionality constant being mass) :  

```math
\sum{F} = m a
```

There is no such thing as mass in Minecraft, so we will considere that acceleration are equal to the sum of all the forces (there is obviously a unit error here, but we can consider forces to be the same as acceleration because mass isn't a thing here).

# Application
Let's apply that to the trajectory of projectiles. First of all we will need to find all the forces applied to a projectile.

We can find these information on the Minecraft wiki ([here](https://minecraft.wiki/w/Entity#Motion_of_entities)), there is only two forces here, one strictly vertical: gravity, and the second one is the drag, being applied on all direction.
We will use the cartesian coordinate system to describe projectiles dynamic (x, y, z, y being the vertical axis on which gravity act). x and z axis being equivalent, we will only describe motion on x axis.

## Formula for gravity and drag

Gravity is the easiest of the two forces, it is denoted as the "acceleration" on the Minecraft wiki, we will call it $W$ (for weight), it is a homogeneous field, which means that gravity is the same everywhere in a Minecraft world, let's put it equal to "-g", the gravitational acceleration : $W = -g$.  
Drag (which can be interpreted as the air resistance, even if it doesn't act exactly the same), is not constant, it is proportional to the velocity of the object, let's call it $D$ and put $D = - d v_t$ where $d$ is what the wiki call the "drag" (**Warning** we will be using d for both the vertical and horizontal motion, but it is not always the same value, beware to change it to the right value for your entity), we use a negative sign because the drag is opposed to th motion.  

## Acceleration and velocity on the x axis

As we said earlier, there is only one force here : the drag, so we have the following equality :  

$$a_t = D = - d v_t$$  

From $a_t = v_{t+1} - v_t$ we get :  

$$- d v_t = v_{t+1} - v_t$$  

$$v_{t+1} = v_t - d v_t$$  

$$v_{t+1} = (1-d) v_t$$  

By a change of variable we get :  

$$v_t = (1-d) v_{t-1}$$  

Now let's try to express $v_t$ with $v_0$ (the initial velocity), because recurrence relation like this one are pretty hard to get around with. We can notice that :  

$$v_t = (1-d) (1-d) v_{t-2}$$  

Or even that :  

$$v_t = v_{t-n} \prod_{k=0}^n{(1-d)}$$  

$$v_t = v_{t-n} (1-d)^n$$  

Let's chose $n=t$ to go back to $v_0$ :  

$$v_t = (1-d)^t v_0$$  

That's the final expression for velocity on x (or z) axis.

## Acceleration and velocity on the y axis

This time we have two forces, gravity and drag, so we have this expression for acceleration :  

$$a_t = W + D = -g - d v_t$$  

We then get :  

$$v_{t+1} = -g - d v_t + v_t$$  

$$v_{t+1} = (1-d) v_t - g$$  

With the same change of variable as earlier we get :  

$$v_t = (1-d) v_{t-1} - g$$  

We also have :  

$$v_t = (1-d) ((1-d) v_{t-2} - g) - g$$  

$$v_t = (1-d) ((1-d) (... ((1-d) v_0 - g) ...) - g) - g$$  

$$v_t = (1-d)^t v_0 - g \frac{1-(1-d)^t}{d}$$  
