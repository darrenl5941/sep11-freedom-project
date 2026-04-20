# Entry 5
##### 4/13/2026

### Engineering Design Process (EDP)

I am currently on _part 7_ of the **EDP**, improving as needed. I have already finished the prototype for my project and tested it for bugs while fixing the major ones. I already know what minor issues I need to fix and what I can do to improve my project and what I can do to go beyond my MVP (_minimal viable project_).

### How I Finished My MVP (Minimum Viable Project)

I have finished my [MVP](https://github.com/darrenl5941/sep11-freedom-project/blob/main/index.html) by adding many more of the major features and replacing placeholder mechanics. This includes things such as `collisionFilters`, `hitboxes`, timed and simultaneous player movement, an end screen, and more different player interactions.

#### Collision Filters and Hitboxes

I added `hitboxes` to multiple of my objects such as the ground and the players to make it so their interactions could be done from further away and the objects did not need to be directly against each other. I made the `hitboxes` slightly larger than the actual objects and made it follow their position so they could detect the collisions instead of the actual object:

```js
// player 1
var p1 = Bodies.rectangle(250, 400, 100, 100, {
    //...
});

// player 1 sensor
var p1Sensor = Bodies.rectangle(250, 400, 140, 140, {
    //...
    // to prevent sensor bodies from sliding down the screen
    inertia: Infinity,
    frictionAir: 0,

    render: {visible: false} // make the body invisible 
});

//...

// Apply movement each tick
Matter.Events.on(engine, "beforeUpdate", (event) => {

    // update `p1Sensor` to `p1` position & prevent it from offsetting itself from velocity
    Body.setPosition(p1Sensor, p1.position);
    Body.setAngle(p1Sensor, p1.angle);
    Body.setVelocity(p1Sensor, { x: 0, y: 0 });
    Body.setAngularVelocity(p1Sensor, 0);

    //...
});
```

The problem with this is that the hixboxes would collide with other objects when I only wanted them to sense when an interaction should be allowed, which is why I also needed to use `collision filters`.

`collision filters` allow me to change which objects are allowed to physically collide with each other, which when combined with `isSensor: true` allows me to make objects that only detect when object are touching but don't actually collide with anything else. I learned how to use `collision filters` through a [a website about phaser](https://reitgames.com/news/collision-filtering-phaser), which uses a similar collision filter system to _matterJS_ and then using an AI, [copilot](https://copilot.microsoft.com/), to help me figure out the small differences in syntax.

Here is an example of the collision filters:

```js
const PLAYER = 0x0001;
const GROUND = 0x0002;
const GHOST  = 0x0004;

// Make bodies here

// ground
var ground = Bodies.rectangle(675, 500, 1000, 20, {
    isStatic: true, // isStatic prevents movement
    label: "ground",
    collisionFilter: {
        category: GROUND,
        mask: PLAYER // players collide with ground
    }
}); 
var groundHitbox = Bodies.rectangle(675, 490, 1050, 50, { // for jumping
    isStatic: true,
    label: "groundSensor",
    isSensor: true, // allows to still detect collisions without actually colliding
    collisionFilter: {
        category: GHOST,
        mask: PLAYER // collide with nothing, but still triggers events with players
    },
    render: {visible: false} // make the body invisible 
});

// player 1
var p1 = Bodies.rectangle(250, 400, 100, 100, {
    friction: 0.01,
    label: "p1",
    collisionFilter: {
        category: PLAYER,
        mask: PLAYER | GROUND | GHOST
    }
});

// player 1 sensor
var p1Sensor = Bodies.rectangle(250, 400, 140, 140, {
    label: "p1Sensor",
    isSensor: true, // allows to still detect collisions without actually colliding 
        collisionFilter: {
        category: GHOST,
        mask: PLAYER // only collide with real player
    },
    // to prevent sensor bodies from sliding down the screen
    inertia: Infinity,
    frictionAir: 0,

    render: {visible: false} // make the body invisible 
});

// same as p1 for p2 except in a different position
```

#### Simultaneous Player Movement

For making both players move at the same time I had to make multiple different `functions` that would chain together. I would have the functions run in different orders for the players turns and then have the movement only apply and objects only move during the `movement phase`. The `functions` and code I made followed an order that looked like this:

1. Player 1's turn
    1. Start turn by pressing space
    2. track time since turn start; `startCountDown()` 
    3. track inputs; `inputFunction(playerInputs, event)`
    4. end turn and change to player 2's turn; `finishCountdown()`
2. Player 2's turn
    1. Start turn by pressing space
    2. track time since turn start; `startCountDown()` 
    3. track inputs; `inputFunction(playerInputs, event)`
    4. end turn and reset for movement phase; `finishCountdown()`
3. Movement Phase; `startMovementPhase()`
    1. Apply forces from inputs `runInputs(player, input)`
    2. Wait for movement to end (0.75 seconds); `Matter.Events.on(engine, "afterUpdate", (event) => {`
    3. Reset to start cycle again; `Matter.Events.on(engine, "afterUpdate", (event) => {`

#### Different Player Interactions

To make the game more interesting I also added multiple different player interactions such as jumping on your opponents head, pressing down to move faster, and slamming down on your opponent to make them smaller.

When jumping on your opponents head I made it so you get to jump like normal, but you push your opponent in the opposite direction compared to how you jump:

```js
function runInputs(player, input) {
    //... down input and normal jump mechanics

    if(input.includes("jumpOnHead")){
        if(player == p1){
            Body.applyForce(p2, p2.position, { x: 1.25 * forceScale, y: 0 });
        } 
        if(player == p2){
            Body.applyForce(p1, p1.position, { x: 1.25 * forceScale, y: 0 });
        }                      
    }
    if (input.includes("right")) {
        Body.applyForce(player, player.position, { x: forceScale, y: 0 });
        // ... other mechanic for down
        // push player in opposite direction if jumping on head
        // on other's head checks
        if ((labels.includes("p1Sensor")) && labels.includes("p2") && (p1.position.y < p2.position.y)) {
            p1OnHead = true;
            // stomping mechanic
            if (phase === "movement" && p1Input.includes("down")) {
                Body.setDensity(p2, p2.density * 0.90);
                Body.scale(p2, 0.90, 0.90); // size change (also affects density/weight)
                Body.scale(p2Sensor, 0.90, 0.90); 
            }
        }
    }
    // ... same for left except opposite numbers
}
```

Pressing down to move faster just applies an extra force in the horizontal direction you move (left or right) to make it easier to use the next mechanic, shrinking your opponent.

```js
function runInputs(player, input) {
    if (input.includes("left")) {
        Body.applyForce(player, player.position, { x: -forceScale, y: 0 });
        // bonus horizontal movement for moving down
        if(input.includes("down")){
            Body.applyForce(player, player.position, { x: -0.25 * forceScale, y: 0 });
        }
        // ... jumping on head mechanic
    }
    // ... same for right
}
```

Slamming down on your opponent works by checking if two conditions are met, you press `down` and your `hitbox` collides with the opponent (which is the `hurtbox`). This mechanic makes it more advantageous for players to try and stay above their opponents or in the air which would only be a negative thing without this mechanic as it also takes away movement options (jumping).

```js
Matter.Events.on(engine, "collisionStart", (event) => {
            event.pairs.forEach(({ bodyA, bodyB }) => {
                const labels = [bodyA.label, bodyB.label];
                // ... other collision checks

                // stomping mechanic
                if (phase === "movement" && p1Input.includes("down")) {
                    Body.setDensity(p2, p2.density * 0.90);
                    Body.scale(p2, 0.90, 0.90); // size change (also affects density/weight)
                    Body.scale(p2Sensor, 0.90, 0.90); 
                }
                // ... same for player 2
            }); 
        });
```

### Skills

#### Debugging

_Debugging_ was one of the things I spent the most time on during the time between the last blog and this one. I had a very large amount of bugs that I had to deal with in this project and most of them would not show in the `console` so I had to manually go and comment out parts of code or use `console.log()` to check which parts were causing the issues like in my `stomping` mechanic which was inside multiple `if` statements and had parts in multiple different `functions`. Knowing how to debug properly is a very important skill when coding as not knowing how to will cause you to spend a large amount of time trying to figure out issues because you don't know what exactly is causing them.

#### Using AI Properly

Knowing how to use AI properly can help you speed up the progress of what you are trying to do while not taking away from your learning and still allowing you to learn how to prevent issues from happening again. An example of this is using AI, [copilot with their study and learn mode](https://copilot.microsoft.com/), to list off the bugs I needed to solve from most important to least important (while not telling me the direct solution to them) so I knew which I should prioritize first:

```
🟥 CRITICAL BUGS (Game‑breaking)
🟥 1. phase was never set to "movement"
Your stomp logic checks:
if (phase === "movement" && p1Input.includes("down"))


But phase never became "movement" anywhere in your code.
This made every stomp check always false, no matter what.

🟥 2. Inputs were cleared BEFORE movement started
Inside your Space handler, you had:
p1Input = [];
p2Input = [];
phase = "input";


This happened immediately after calling startMovementPhase().
So when collisions happened:
- "down" was already removed
- stomp could never trigger
This was the single biggest reason "2.1" and "2.2" never printed.

🟥 3. Movement ended instantly on turn 2+
Because movementTime was not reset at the end of movement, the next turn saw:
movementTime >= 750


immediately, so movement ended instantly.
This prevented:
- collisions
- stomp
- physics
- movement
from happening at all.

🟧 MAJOR LOGIC BUGS (Breaks turn flow)
🟧 4. Reset logic was in the wrong place
You reset:
- p1Ready
- p2Ready
- pCurrent
- phase
- p1Input
- p2Input
before physics ran.
This caused:
- no movement
- no stomp
- broken turn order
- countdown starting at the wrong time

🟧 5. Countdown logic was unreachable
You had:
return; // prevent count down phase

if (countDownStored == null) {
    startCountDown();
}


Everything after return never executed.
So countdown sometimes didn’t start at all.

🟨 MINOR / SIDE‑EFFECT BUGS (Don’t break stomp but cause weird behavior)
🟨 6. Sensors were correct, but stomp timing was impossible
Your collision logic was fine, but because:
- phase was wrong
- inputs were cleared
- movement ended instantly
the stomp mechanic never had a chance to run.

🟨 7. Density‑scaled forces were too small
Using:
forceMagnitude * player.density


produced forces like:
0.00025

which are too tiny to move anything.
(Not directly related to the stomp bug, but it made movement feel broken.)
```




<!-- Text

[Previous](entry04.md) | [Next](entry06.md)

[Home](../README.md) -->