# Entry 4
##### 3/9/1026

### Engineering Design Process (EDP)

I am currently on _part 5_ of the **EDP**, creating the prototype. I have already leanred most of the tools I need to know in my tool, `matterJS`, for my project and have already planned how to make my project so I have started working the creating the **Minimal Viable Product**, or _MVP_.

### What I have Continued Learning About my Tool

Although I have already started working on creating my _MVP_ I have also still been learning more about `matterJS`. I have learned how to make `pairs` which allow me to check for collisions are run events based off of them using the `collisionStart` and `collisionEnd` events. This will allow me to have more specific interactions between objects when the collide, such as being able to make it so an object can only _jump_ when it is touching the ground.

#### Example of using this to make jumping

```js
// checks if boxA is touching the ground
let onGround = false;
Matter.Events.on(engine, "collisionStart", (event) => { // Checks for when objects initially collide
    event.pairs.forEach((pair) => {
        const a = pair.bodyA;
        const b = pair.bodyB;

        // if the colliding objects are the player controlled object and the ground then the varibale tracking if they can jump `onGround` is true, allowing them to jump in another function
        if ((a.label == "boxA" && b.label == "ground") || (b.label == "boxA" && a.label == "ground")) { // check for it both ways; if bodyA and bodyB are swapped
            onGround = true;
        }
    });
});
Matter.Events.on(engine, "collisionEnd", (event) => { // Checks for when objects stop touching
    event.pairs.forEach((pair) => {
        const a = pair.bodyA;
        const b = pair.bodyB;

        // if the objects that stop colliding are the player controlled object and the ground then the varibale tracking if they can jump `onGround` is false, preventing them from jumping in another function
        if (!(a.label == "boxA" && b.label == "ground") || !(b.label == "boxA" && a.label == "ground")) { // check for it both ways; if bodyA and bodyB are swapped
            onGround = false;
        }
    });
});
```

### 

[Previous](entry03.md) | [Next](entry05.md)

[Home](../README.md)