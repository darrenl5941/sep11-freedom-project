# Entry 4
##### 3/9/1026

### Engineering Design Process (EDP)

I am currently on _part 5_ of the **EDP**, creating the prototype. I have already leanred most of the tools I need to know in my tool, `matterJS`, for my project and have already planned how to make my project so I have started working the creating the **Minimal Viable Product**, or _MVP_.

### What I have Continued Learning About my Tool

Although I have already started working on creating my [MVP](https://github.com/darrenl5941/sep11-freedom-project/blob/main/index.html) I have also still been learning more about `matterJS`. I have learned how to make `pairs` which allow me to check for collisions are run events based off of them using the `collisionStart` and `collisionEnd` events. This will allow me to have more specific interactions between objects when the collide, such as being able to make it so an object can only _jump_ when it is touching the ground.

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

### Progress to MVP

While making my MVP I have been following the [plan](https://github.com/darrenl5941/sep11-freedom-project/blob/main/prep/plan.md) I made:

```md
#### MVP

- [X] Make two different controlable shapes (deadline: 3/19)
  - [X] Create the swapping for which is being controlled (deadline: 3/23)
- [ ] Add the timed pausing for between turns/moves (deadline: 3/25)
  - [ ] Make sure forces or other things are not being applied while it is paused (deadline: 3/27)
  - [ ] Prevent inputs from happening when not paused (deadline: 3/30)
- [ ] Make match end when one player is knocked off (deadline: 3/30)
- [ ] Add different moves other than just basic movement (deadline: 4/1)
  - [ ] Balancing (deadline: 4/2)
- [ ] Create size/density changes for damage (deadline: 4/3)
```

So far I have created some of the basic and mose essential parts for my MVP.

I have made two different controbale player objects and swapping between them using `event listeners` checking for key inputs:

```js
// Apply movement each tick
Matter.Events.on(engine, "beforeUpdate", () => {
    const forceMagnitude = 0.01; // amount of force when key is pressed
    let currentGrounded = pCurrent.label === "p1" ? p1onGround : p2onGround; // checks which is current object so I don't have to write the arrow up function seperatley for both

    if (keys["ArrowUp"] && currentGrounded) {
        Body.applyForce(pCurrent, pCurrent.position, { x: 0, y: -50 * forceMagnitude });
    }

    if (keys["ArrowDown"]) {
        Body.applyForce(pCurrent, pCurrent.position, { x: 0, y: forceMagnitude });
    }

    // ...

    if (keys["Space"] && !spacePressed) {
        spacePressed = true; // prevents constant toggling
        if(pCurrent == p1){
            pCurrent = p2
        }
        else if(pCurrent == p2){
            pCurrent = p1
        }
        // run  = !run ;
        // runner.enabled = run;
        // console.log(run)
    }
    else if (!keys["Space"]) {
        spacePressed = false; // resets `spacePressed`
    }
});
```

When one of the _arrow keys_ on the keyboard are pressed it moves the object and pressing space swaps which object is being controlled.

To make jumping I had to check if the current player was on the ground so they wouldn't be able to jump while in the air by using `pairs` and checking for `collisions`:

```js
// checks if current player is touching the ground
let p1onGround = false;
let p2onGround = false;
Matter.Events.on(engine, "collisionStart", (event) => {
event.pairs.forEach(({ bodyA, bodyB }) => {
    const labels = [bodyA.label, bodyB.label];
        if (labels.includes("p1") && labels.includes("ground")) {
            p1onGround = true;
        }
        if (labels.includes("p2") && labels.includes("ground")) {
            p2onGround = true;
        }
    }); 
});

Matter.Events.on(engine, "collisionEnd", (event) => {
    event.pairs.forEach(({ bodyA, bodyB }) => {
        const labels = [bodyA.label, bodyB.label];
        if (labels.includes("p1") && labels.includes("ground")) {
            p1onGround = false;
        }
        if (labels.includes("p2") && labels.includes("ground")) {
            p2onGround = false;
        }
    });
});
```

[Previous](entry03.md) | [Next](entry05.md)

[Home](../README.md)