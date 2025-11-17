# Tool Learning Log

## Tool: **MatterJS**

## Project: **X**

---

### 10/5/2025:
* **Module aliases** are things that set things that are commonly used to different names.
  * Ex:
  ```js
    var Engine = Matter.Engine,
        Render = Matter.Render,
        Runner = Matter.Runner,
        Bodies = Matter.Bodies;

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

### 10/27/2025:

* **When making a box/rectangle** the first two values control the starting x and y value while the last two values control the width and height of the shape.
  * ex:
  ```js
  // this would create a box 400 pixels to the right and 200 pixels down that is 80 pixels wide and 80 pixels tall
  var boxA = Bodies.rectangle(400, 200, 80, 80);
  ```
* You can **drag objects** by creating a mouse with `Mouse.create(render.canvas)` which allows you to drag objects with your mouse
  * **Stiffness** changes not close the object stays to the mouse or how sticky it is to the mouse
  * `visible: true` allows you to see the connection from the mouse to the elements.
* By using an `EventListener` that checks for if a button is pressed I can make it **add more shapes** to the simulaton.
  * By putting in `Math.random()` for the height and width I can make it generate a different sized box each time

### 11/10/2025:

* `Constraint.create()` lets you connect two objects or points
  * Useful for making things have a limit on how far they can go or for making objects hang
* `Contraint.chain()` lets you connect **multiple** objects at once
  * example of code:
  ```js
  Composites.chain(bridge, 0.3, 0, -0.3, 0, {
    stiffness: 0.99,
    length: 0.0001,
    render: {
        visible: false
    }
  });
  ```
  * `stiffness` and `length` don't seem to do anything why they are changed, but it breaks if stiffness is above _1_
  * The _first_ parameter, in the case above `bridge`, is the group of objects that you want to connect
  * The _second_ and _fourth_ parameter control the distance between objects
    * Objects can be pulled past that distance with enough force
  * The _third_ and _fifth_ parameter control how hard the objects are pulled together


<!-- ### X/X/XX:
* Text
 -->

<!--
* Links you used today (websites, videos, etc)
* Things you tried, progress you made, etc
* Challenges, a-ha moments, etc
* Questions you still have
* What you're going to try next
-->
