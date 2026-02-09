# Entry 3
##### 2/2/2026

### Engineering Design Process (EDP)

I am currently still on _part 2_ of the EDP, researching the problem. I learned more of how to _control interactions between different objects and their surroundings by using `properties`. By using properties I can control many things such as how much objects naturally slow down, their gravity, and how heavy the objects are. This will allow me to fine-tune how I want the objects in my project to react.

### The Different Properties I Learned

* `density`, which allows you to _change the weight_ of an object
* `.gravity`, which pulls all objects in a specified direction with a specified and _constant amount of force_
* `friction`, which controls the _speed an object slows_ down when in contact with another object
    * `frictionStatic`, which changes _how hard it is to push an object_ or make it start moving
    * `frictionair`, which changes how much _air resistance_ or friction in the air an object has

### Using the new Properties

#### Density

The first property I used was `density`, which I learned from the [matterJS website](https://brm.io/matter-js/docs/classes/Body.html). `density` allowed me to _change how objects would interact with each other or with forces_. Lowering the density  allows for objects to gain more velocity from forces applied on them and causes the object to be pushed more when having a collision with another object. Raising the `density` has the opposite effect.

Here is an example of density being used to make an object lighter:

```js
var boxB = Bodies.rectangle(400, 200, 80, 80,
    {
        density: 0.002, // changes the weight of the object
        //...
    }
);
```

#### Gravity
Another property, `gravity`, allows you to _apply a constant force_ on all objects. It can be used in more directions than just pulling objects downward. To change the gravity you need to use `engine.world` first so you apply it to the whole simulation.

Here is an example of changing the gravity to make objects float in the air longer:

```js
engine.world.gravity.y = 0.2; // pulls objects down slowly, low gravity
```

#### Friction

I learned about `friction` from [this youtube video](https://www.youtube.com/watch?v=urR596FsU68&t=27m7s). In that video I learned that `friction` allows you to _control how much objects will slow down_ while they are moving across the ground or another object. There are also other versions of friction such as `frictionStatic` which controls how hard it is to make an object start moving and `frictionAir` which controls the air resistance for the object. If you set the `friction` to 0 but not the `frictionStatic` or `frictionAir` the object will still slow down over time.

here is an example of using `friction` and the other friction properties:

```js
var boxA = Bodies.rectangle(400, 200, 80, 80,
    {
        // setting them all to 0 means the object will never stop or slow down unless it collides with another object.
        friction: 0.0, // normal friction, also affects how much object slows down
        frictionStatic: 0.0,  // how hard an object is to push
        frictionAir: 0.00     // friction when the object isn't touching anything or is in the air
    }
);
```

### Skills

#### Researching

One of the skills I learned was how to _research information_. While I was researching I used a variety of sources and different types of sources such as _youtube videos_ and the _official website_ for matterJS. By not relying on just one source I was able to learn a lot more and a larger variety of properties more efficiently than if I were to only use one source.

#### Debugging

Another one of the skills I learned was how to _debug and problem solve_. While using the `friction` property I noticed that the object would still slightly slow down even when the friction was set to 0, so I tested and searched for solutions which led to me figuring out that other properties such as `frictionStatic` and `frictionAir` still slow down the object even when normal friction is 0. Debugging and problem solving is an important skill because it allows you gain a deeper understanding of what you are learning by solving issues that you didn't even know existed before you had to start debugging.

[Previous](entry02.md) | [Next](entry04.md)

[Home](../README.md)
