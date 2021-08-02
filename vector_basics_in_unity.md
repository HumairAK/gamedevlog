# Vector3 Basics (Position, Distance, Direction)

This document describes vectors from the perspective of unity.  We will cover vectors in 3D cartesian space. For unity this means [Vector3](https://docs.unity3d.com/ScriptReference/Vector3.html). 

Much of this is probably applicable to Vector2 as well.


### What is a Vector3?

A Vector3 can be thought of as a point in 3d space that comprises of both a direction and a magnitude. 

#### Direction

Try not to confuse direction with rotation, as these are different things. A vector has a direction, an object has a rotation. A direction for a vector can only be understood relative to another point in space. Let's take an example:

```c#
Vector3 playerOrigin = transform.position;
Vector3 direction = playerOrigin.normalized;
```

 In this example the [normalized](https://docs.unity3d.com/ScriptReference/Vector3-normalized.html) call gives the direction of the `player` from the world origin. Why world origin? Because remember that we are looking at the vector that is `transform.position`, the position of an object in this case is in world space, meaning everything is relative to the origin.

 `normalized` gives you a vector with a magnitude of 1. This basically means that the "distance" portion of the vector is stripped away. 



![](.\assets\vector_basics_in_unity\1.PNG)

In the illustration above, the red is the origin, and the green cube is the player. Then `direction` from the code snippet above represents the arrow. 

#### Magnitude 

The magnitude of a vector can either be thought of as the distance or force. Let's think of them as distances for now. Like direction, distances are only meaningful relative to another point. 

```c#
Vector3 playerOrigin = transform.position;
float playerDistance = playerOrigin.magnitude;
```

Similar to distance, in this example the `magnitude` can be thought of as the distance of the player from the origin. This is illustrated below: 

![2](.\assets\vector_basics_in_unity\2.PNG)





Let's prove that we're right in this case by drawing a line from the origin (red sphere) to the player (green cube) by adding the following script to one of the objects: 

```c#
    public GameObject origin;
    public GameObject player;
    void Update()
    {
        Vector3 playerPos = player.transform.position;
        Vector3 originPos = origin.transform.position;

        Vector3 directionFromOriginToPlayer = playerPos.normalized;
        
        Debug.DrawRay(
            originPos,
            directionFromOriginToPlayer,
            Color.blue
        );
```

In this script we draw a line from the origin to the direction of the player, provided that `normalized` indeed gives us this direction. Running the script results in: 

![](F:\Blog\gamedevlog.github.io\assets\vector_basics_in_unity\1.gif)

Indeed we get the results we expect. The blue ray is indeed going from the origin to the direction of the player. But why is the line not connecting with the player? Remember that the `normalized` call simply returns another vector with a magnitude of 1, in this case magnitude represents distance, so the blue line only has a distance of 1. To fully connect this to the player, we need to incorporate the vector distance (magnitude) as well! 



```C#
        ... // same as before
        Vector3 directionFromOriginToPlayer = playerPos.normalized;
        float distanceFromOriginToPlayer = playerPos.magnitude;
        
        Debug.DrawRay(
            originPos,
            directionFromOriginToPlayer * distanceFromOriginToPlayer,
            Color.blue
        );
```

By multiplying the direction by distance, we are "scaling" the direction vector by that distance value, this can be conceptually thought of as _moving in that direction_. The amended code gives the following result: 

![](.\assets\vector_basics_in_unity\2.gif)

Success! Note that we will see the exact same results if we replace `playerPos` with `directionFromOriginToPlayer * distanceFromOriginToPlayer`. This is expected, since the whole point of the exercise is to show that the position vector3 is, in fact, comprised of these two properties: direction & distance. 

Note that both the `Magnitude` and `Distance` were extracted from the vector3 found from the `transform.position` . This means that the `transform.position` which is just a vector3 of the form (x,y,z) is more than just some point in space, but retains both these properties. But it's always important to remember that these properties only have meaning when considering another point in space (in this case the origin).  

#### Distance & Magnitude relative to another Object 

So far we have been considering only position relative to the origin for the player. But what if instead of the origin, we have a different object. In other words, what about another object relative to our player?  



![3](.\assets\vector_basics_in_unity\3.png)

In this new example we have another object, the blue cube. Let's call this blue cube the "enemy". As the illustration suggests, what if we want to calculate the distance from the player and the enemy? same with direction. Before the distance/direction from origin to player was implicit by the call to `player.transform.position`. We can get a similar result for the enemy player by doing something like `enemy.transform.position` like so: 

```c#
    public GameObject enemy;
    public GameObject origin;
    public GameObject player;
    void Update()
    {
        Vector3 playerPos = player.transform.position;
        Vector3 enemyPos = enemy.transform.position;
        Vector3 originPos = origin.transform.position;
        
        Debug.DrawRay(
            originPos,
            playerPos, // now that we know playerPos is distance*direction (from origin) we can directly use playerPos
            Color.blue
        );
        
        Debug.DrawRay(
            originPos,
            enemyPos,
            Color.green
        );
```

This results in: 

![](.\assets\vector_basics_in_unity\3.gif)



Great, but we want to draw a line from the player to the enemy (blue cube). What happens if we simply add: 

```c#
        Debug.DrawRay(
            playerPos,
            enemyPos,
            Color.red
        );
```

Take a minute to think about why this won't work. 



Okay so let's see what happens if we run this: 

![](F:\Blog\gamedevlog.github.io\assets\vector_basics_in_unity\4.gif)



What is going on here? Remember that the second argument to [DrawRay](https://docs.unity3d.com/ScriptReference/Debug.DrawRay.html) takes a `direction`, but a direction is always relative to another point in space right? So we know from earlier `enemyPos` comes from `enemy.transform.position` and `position` of a transform contains the direction/distance relative to... the origin! So all we've done here is draw a line from the player in the direction of origin->enemy, which is why the red line is essentially _parallel_ to the green. 

What we need is to find the direction of the player to the enemy. This requires us to perform some Vector arithmetic. To get a direction of a vector A to vector B we need to subtract vector B from vector A i.e. `AB-> = B - A` . It' easier to explain why this works if we consider 2D vectors and the following diagram of a 2d cartesian space: 

![](.\assets\vector_basics_in_unity\4.png)

I've kept the color scheme of the lines analogous to our example. The red vector, which is (3, -2) gives the directionfrom the point (2,4) to (5,2). Think of it it as the "steps" one needs to take to get to point (2,4) to (5,2). In this case we need to go 3 steps right, and 2 steps down from 2,4 to get to 5,2. If we instead subtract `(2,4) - (5,2)` then we get the reverse i.e. (-3, 2), 3 steps left, 2 steps up. Which is the vector moving from (5,2) in the direction towards (2,4). 

Let's use this new found knowledge in our example: 


```C#
        Vector3 playerToEnemy = enemyPos - playerPos;  

		// playerToEnemy is made up a direction and distance relative to the enemy
        Vector3 directionFromPlayerToEnemy = playerToEnemy.normalized;
        float distanceFromPlayerToEnemy = playerToEnemy.magnitude; 
        
        Debug.DrawRay(
            playerPos,
            directionFromPlayerToEnemy * distanceFromPlayerToEnemy,  // same as if we just put playerToEnemy
            Color.red
        );

```

This gives us the following result: 

![](.\assets\vector_basics_in_unity\5.gif)

And we get the intended result! 

