# Tool Learning Log

## Tool: **MatterJS**

## Project: **X**

---

### 10/5/25:
* **Module aliases** are things that set things that are commonly used to different names.
  * Ex:
  ```js
    var Engine = Matter.Engine,
        Render = Matter.Render,
        Runner = Matter.Runner,
        Bodies = Matter.Bodies,

    function setup(){
        createCanvas(400, 400);
    }
  ```
* **Bodies:** Allows you to make variables for shapes/objects.
  * **.rectangle:** Makes a rectangle (x-position, y-position, width, height).
* **{ isStatic: true }:** Makes it so the object is static/doesn’t move.
* **Runner.run:** Runs the engine.
* **`Composite.add(engine.world, [“objects”])`:** Adds the specified objects into the world.
  * Ex:
  ```js
  Composite.add(engine.world, [boxA, boxB, ground]);
  ```
  This would add the shapes `boxA`, `boxB`, and `ground` into the world.

### X/X/XX:
* Text


<!--
* Links you used today (websites, videos, etc)
* Things you tried, progress you made, etc
* Challenges, a-ha moments, etc
* Questions you still have
* What you're going to try next
-->
