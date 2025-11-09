# Entry 1: Choosing a tool (Matter JS)
##### 11/03/2025

### Why I chose Matter JS

Before I chose my tool I had to think of what I wanted to make, which was a game which used physics as its main mechanic. Once I decided on what I wanted to make I looked at and tinkered with multiple different tools that could help me make the physics part such as [_P5play_](https://p5play.org/). I decided on using [_Matter JS_](https://brm.io/matter-js/) as it focused on physics and was the best choice for what I wanted to do with the most options on what mechanics I could make.

### Engineering Design Process (EDP)

I am currently on _steps 1 and 2_ of the EDP (define and research the problem). I have found what I wanted to make (a physics based fighting game), and I have started researching and testing tools which can help me to make it.

### Tinkering with Matter JS

Once I decided that I wanted to use Matter JS I started to tinker with it more. The first thing I did was make a simple box with collisions:

```js
var box = Bodies.rectangle(200, 200, 100, 40); // makes a variable for the box and says where it should be and its size

World.add(engine.world, [ground, box]) // adds the objects on the screen
```

Once I did that and was able to make simple objects I tried to make it more interactive by allowing the objects to be _dragged by the mouse_. A problem I had with trying to do this was that there were no guides that would show how to do it, so I used AI to ask for what I would need to make it and it told me to use `mouse.create` as a _input handler_ and `MouseConstraint.create` as an _interaction constraint so I could say how I wanted the mouse to work. With that information I was able to make the mouse able to drag objects with the code below:

```js
const mouse = Mouse.create(render.canvas);
const mouseConstraint = MouseConstraint.create(engine, {
    mouse: mouse, // makes the object dragging the objects the mouse
    constraint: {
        stiffness: 0.1, // changes how much force is used to drag the object to the mouse
        render: {
            visible: true // lets you see the object's connection to the mouse
        }
    }
});
```

By changing the `stiffness` in the code to a higher value I could make the object follow the mouse more closely and by lowering it the object would have a slight delay and move around the mouse before staying with it. By lowering the stiffness to an extremely low value like `0.0001` I could get the object to hang and swing below the mouse.

### Skills

#### Creativity

One of the skills that I learned through this was to _think outside the box and be more innovative_. Most of the guides for _Matter JS_ just went over the basics like how to make the world and basic objects, so I had to be creative and think of different things that I wanted to do with _Matter JS_ and then research those things specifically. An example of this is when I searched [_matter js guide_](https://www.google.com/search?q=matter+js+guide&sca_esv=a4bf60fddd2c2458&rlz=1C1CHBF_enUS904US904&udm=7&biw=1908&bih=1464&ei=yOkQaYmDN7nY5NoP-aKW0Aw&ved=0ahUKEwiJh5ea5eWQAxU5LFkFHXmRBcoQ4dUDCBE&uact=5&oq=matter+js+guide&gs_lp=EhZnd3Mtd2l6LW1vZGVsZXNzLXZpZGVvIg9tYXR0ZXIganMgZ3VpZGUyBBAAGB4yCBAAGIAEGKIEMggQABiABBiiBDIIEAAYgAQYogQyCBAAGIAEGKIESKwKUK4FWK4FcAF4AJABAJgB_AGgAfwBqgEDMi0xuAEDyAEA-AEBmAICoAKCAsICBhAAGA0YHpgDAOIDBRIBMSBAiAYBkgcFMS4wLjGgB9YCsgcDMi0xuAf_AcIHAzAuMsgHBA&sclient=gws-wiz-modeless-video&safe=active&ssui=on) all the videos would just show the same things. So I came up with the idea of using the mouse to drag objects and then searched up [matter js how to drag objects with mouse](https://www.google.com/search?sca_esv=a4bf60fddd2c2458&rlz=1C1CHBF_enUS904US904&udm=7&fbs=AIIjpHxU7SXXniUZfeShr2fp4giZ59Aj-dkSgmXWKpa2HWaBZFiTO0ouTWmFCwDJPeX-aDO4uZSPDi8pKZ8j61k5i7gzDpMnef6z8OSSevFSq66hqaEQHrfPI314WweCk6E29EHHWBXrSmPrROl2wVv8rAPYuGPHAe_XTeKCG3a7440fzZ-C32AHHgSqVY54x_N1OXR29YieHrtVZ8KpLK85Tq0exryThw&q=matter+js+how+to+drag+objects+with+mouse&sa=X&ved=2ahUKEwj71JX85eWQAxVwEmIAHcqmJU4QtKgLegQIHhAB&biw=1908&bih=1464&dpr=0.9&safe=active&ssui=on) which gave me a [video](https://www.youtube.com/watch?v=W-ou_sVlTWk) on how to do that.

#### How to Research

Another skill I learned was how to research because many of the guides I used were old and had _outdated code_. When I tried to look for more recent guides there were none, so I used AI to research for what the newer versions used. An example of this is when I had a line of code that was using `World.add` which didn't work, so I asked the AI what the newer version was and it told me to use `Composite.add` which fixed the problem. I learned that AI can be a helpful when you use it to find the tools and resources for what you are trying to make and not using it to give you and answer directly.

[Next](entry02.md)

[Home](../README.md)
