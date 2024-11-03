# Tool Learning Log

## Tool: **Kaboom**

## Project: **Tower PVE battle game**

---
## Kaboom.js (game engine)

First I watch a video on youtube to get some idea about how to start and this [video](https://www.youtube.com/watch?v=r9I4DuGmJ2Y) help me get started





Basic code
```js
// start the game
kaboom()

// define gravity
setGravity(2400)

// load a default sprite
loadBean()

// add character to screen, from a list of components
const player = add([
    sprite("bean"),  // renders as a sprite
    pos(120, 80),    // position in world
    area(),          // has a collider
    body(),          // responds to physics and gravity
])

// jump when player presses "space" key
onKeyPress("space", () => {
    // .jump() is provided by the body() component
    player.jump()
})
```
# 10/28
First I watch this [video](https://www.youtube.com/watch?v=n-q0pKGhxyw) to get an basic idea about the basic of sprite.

## Creating a Sprite
To create a sprite use the add function along with the sprite method. Here is an example:

```js
kaboom(); // Initialize Kaboom

const player = add([
    sprite("player"), // load a sprite named "player"
    pos(100, 100),    // set position
    area(),           // collision area
    body(),           // physics
]);
```

Before using a sprite, you need to load it. This is how it's done at the beginning:

```js

loadSprite("player", "path/to/player.png");

```
## Animating Sprites
You can animate a sprite by creating animations:

```js
loadSprite("player", "path/to/player.png");
loadSprite("player_run", "path/to/player_run.png");

anim("run", {
    frames: [
        "player_run1",
        "player_run2",
    ],
    speed: 10,
    loop: true,
});

// Set the animation on the sprite
player.play("run");
Moving Sprites

```

To move a sprite, you can use the move() method or respond to input:

```js

player.move(100, 0); // Move right

// Or with input
onKeyPress("right", () => {
    player.move(200, 0);
});

```

### Summary
Create sprites with add and sprite. Load sprites using loadSprite. Animate with play. And I'm going to try to add collision detection next.








<!-- 
* Links you used today (websites, videos, etc)
* Things you tried, progress you made, etc
* Challenges, a-ha moments, etc
* Questions you still have
* What you're going to try next
-->


## 11/3/24


Collision Detection 

To handle collisions, you can use the collides method: 

```js

player.collides("enemy", () => {
    // Handle collision with an enemy
    console.log("Ouch!");
});

// Here's a simple example


kaboom();

loadSprite("player");
loadSprite("enemy");

const player = add([
    sprite("player"),
    pos(100, 100),
    area(),
    body(),
]);

const enemy = add([
    sprite("enemy"),
    pos(200, 100),
    area(),
]);

player.onCollides("enemy", () => {
    console.log("Ouch!");
});

keyPress("right", () => {
    player.move(200, 0);
});


```

## Platform
```js
add([
    rect(width(), 48), // renders a rectangle. It accepts 2 arguments, the width and height, which we give it the game width (returned by width()) and height of 48 pixels

    pos(0, height() - 48), // pos() position. We give it a x: 0 and y: height() - 48 so it sits right on the bottom of the screen
    outline(4), // renders an outline of 4 pixels
    area(), //  adds a collider to it
    body({ isStatic: true }), // the object won't move, and all non static objects won't move past it
    color(127, 200, 255),
])

```

## move 

```js

SPEED = // the speed you want

onKeyDown("left", () => {
	player.move(-SPEED, 0) // moving left you add a "-" sign
})

onKeyDown("right", () => {
	player.move(SPEED, 0)
})

onKeyDown("up", () => {    
	player.move(0, -SPEED)     // moving up or down place the 0 on the x-axis
})

onKeyDown("down", () => {
	player.move(0, SPEED)
})

```

## summery 

I thinker with all my code and created a working sprite that's movable and have collision and when It hit the image it console.log message.



