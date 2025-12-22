# Entry 2: Practicing MatterJS
##### 12/15/2025

### What I Focused on Practicing

While learning _MatterJS_ I focused on practicing with things that would help with _player input_ or _moving and changing objects_. The reason I focused on these things was because they would help me with the game I am planning to make for my _freedom project_.

### Engineering Design Process (EDP)

I am currently on _part 2_ of the EDP, researching the problem. I am currently researching my tool, _Matter JS_, and learning the different functions and experimenting with the different things I can do with it in preperation for using it in my project.

### Experimenting with Matter JS

While experimenting with _Matter JS_ I learned many different unique things that you can do with it such as constrainints, connecting or chaining objects, pausing the simulation, user inputs, and more.

#### Constraints and Chains

_Constraints_ and _chains_ are very useful as they allow you to _connect objects to specific points or other objects_. One use of this can be seen if you are making a bridge like in the bridge example on the [matter-js github page](https://github.com/liabru/matter-js/blob/master/examples/bridge.js).

Using `Constraint.create()` allows you to connect objects to a specific point which prevents them from moving away from that point of pulls them to it. Here is an example of it:

```js
Constraint.create({
    pointA: { x: 140, y: 300 }, // controls point of constraint
    bodyB: bridge.bodies[0], // what object is connected to the constraint
    pointB: { x: -25, y: 0 }, // breaks the contraint if you change the values too much
    length: 2, // how far the object can go from the constraint normally
    stiffness: 0.9 // how much it will resist the object being pulled away
})
```

`Composites.chain()` allows you to make a chain of objects that are all connected to eachother. THe _first parameter_ is the group of objects, the _second_ and _fourth_ are the distance between the objects in the group, and the _third_ and _fifth_ parameters control how hard the objects are pulled together. Here is an example:

```js
Composites.chain(bridge, 0.3, 0, -0.3, 0, {
    stiffness: 0.99, // controls the stiffness of the chain's bonds
    length: 0.0001, // controls how long the bonds between the objects in the chain are
    render: {
        visible: false
    }
});
```

#### Pausing the Simulation

You can pause the simulation by changing `runner.enabled` to false, causing the engine to stop running untill it is re-enabled. Using this you can make a pause button like this:

```js
let pause = false;

// When the spacebar is pressed it stopps/starts the engine
document.addEventListener('keydown', function(event) {
    if (event.key === ' ') {
        pause = !pause;
        runner.enabled = pause;
    }
});
```

#### Controling Objects with User Input

You can control the movement of objects with `forceMagnitude`, which puts a force on an object. Combining this with _event listeners_ allows you to make user inputs put a force on objects. Here is an example of it:

```js
const keys = {}; // Track which keys are down

document.addEventListener("keydown", (event) => { // code works when key is down
    keys[event.code] = true;
});

document.addEventListener("keyup", (event) => { // code stops working when you stop pressing the key
    keys[event.code] = false;
});

// Apply movement each tick
Matter.Events.on(engine, "beforeUpdate", () => {
    const forceMagnitude = 0.01; // amount of force when key is pressed

    if (keys["ArrowUp"]) { // when up arrow is held down
        Body.applyForce(boxA, boxA.position, { x: 0, y: -forceMagnitude }); // puts the force on the shape consistently

// ... more code below, same as if statement for `ArrowUp` but for other keys
```

### Skills

#### Organization

One of the skills I learned from this is _organization_ when I commented my code and kept track of it in my [learning log](https://github.com/darrenl5941/sep11-freedom-project/blob/main/tool/learning-log.md). Keeping your things organized allows you to quickly look through what you did before and understand what it was for. An example of this being useful can be seen with the comments and notes on all of the code I wrote allowing me to understand what the code I made previously does, allowing me to write this blog about it easily.

#### How to Google

Another one of the skills I learned was _how to google_. When I was coding my object movement with player inputs it would apply the force once, then continue applying it after a few seconds of holding down. By googling the issue and what was wrong with my code I figured out that the code would only run while the _event lisener_ was initially used, meaning that it would not check for if the key was held down or not and by googling that I managed to save a large amount of time and figure out what I needed to do to solve the issue. Like in that case knowing how to google is very useful as it allows you to quickly figure out where the specific issue is and what is causing the issue, saving a large amount of time.

[Previous](entry01.md) | [Next](entry03.md)

[Home](../README.md)
