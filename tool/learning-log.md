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







<!-- 
* Links you used today (websites, videos, etc)
* Things you tried, progress you made, etc
* Challenges, a-ha moments, etc
* Questions you still have
* What you're going to try next
-->
