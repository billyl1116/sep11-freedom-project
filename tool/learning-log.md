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


