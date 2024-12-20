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


## 11/11/24




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


## summary


I thinker with all my code and created a working sprite that's movable and have collision and when It hit the image it console.log message. In [kaboom.html](../kaboom.html)








## 11/24/24


I made a working parkour game that shows score and has an ending screen.


```js
function jump() {
        if (player.isGrounded()) {
            player.jump(JUMP_FORCE) // make it so player can't continually jumping
        }
    }


    // jump when player press space
    onKeyPress("space", jump)
    onClick(jump)
```
this make the basis for the player to jump so they can escape the object


---


and to make the object you do this


```js
function spawnTree() {


        // add tree obj
        add([
            rect(48, rand(32, 96)),
            area(),
            outline(4),
            pos(width(), height() - FLOOR_HEIGHT),
            anchor("botleft"),
            color(238, 143, 203),
            move(LEFT, SPEED),
            offscreen({ destroy: true }),
            "tree",
        ])


        // wait a random amount of time to spawn next tree
        wait(rand(0.5, 1.5), spawnTree)


    // start spawning trees
    spawnTree()


    player.onCollide("tree", () => {
        // go to "lose" scene and pass the score
        go("lose", score)
        burp()
        addKaboom(player.pos)
    })


```
this make it so the object will move from right to left and spawn object at certain place and when the player collide with the object they will lose.


---


and to add score you depend on the amount of time they survive
```js
    let score = 0


    const scoreLabel = add([
        text(score),
        pos(24, 24),
    ])


    // increment score every frame
    onUpdate(() => {
        score++
        scoreLabel.text = score
    })


})
```
This will increase the score by the time they live.


---


```js


scene("lose", (score) => {


    add([
        sprite("bean"),
        pos(width() / 2, height() / 2 - 64),
        scale(2),
        anchor("center"),
    ])


    // display score
    add([
        text(score),
        pos(width() / 2, height() / 2 + 64),
        scale(2),
        anchor("center"),
    ])


    // go back to game with space is pressed
    onKeyPress("space", () => go("game"))
    onClick(() => go("game"))


})


go("game")
```


This is how you make the ending screen and what you can do to restart and loop the game so players don't have to refresh.


## summary


I learn how to make a parkour game and this is how i made it in [kaboom2.0.html](../kaboom2.0.html)






<!--
* Links you used today (websites, videos, etc)
* Things you tried, progress you made, etc
* Challenges, a-ha moments, etc
* Questions you still have
* What you're going to try next
-->


## 12/8/24


I made a working npc that will "move", "still", and "attack"


first you set the value of the speed


```js
const SPEED = 320
const ENEMY_SPEED = 160
const BULLET_SPEED = 800
```


after having the value of the speed you can create a npc


```js
const enemy = add([
    sprite("enemy"),
    pos(width() - 80, height() - 80),
    anchor("center"),
    // the enemy cycle between 3 states, and start from "idle" state
    state("move", [ "idle", "attack", "move" ]),
])
```
this create the npc with three different state, and with that we can create the first state `idle` like this


```js
enemy.onStateEnter("idle", async () => {
    await wait(0.5)
    enemy.enterState("attack")
})
```
this will make the npc enter `idle` with for 0.5 second or you can change the time to whatever you want, and it will attack.


```js
enemy.onStateEnter("attack", async () => {


    // Don't do anything if player doesn't exist anymore
    if (player.exists()) {


        const dir = player.pos.sub(enemy.pos).unit()


        add([
            pos(enemy.pos),
            sprite("bullet"),
            move(dir, BULLET_SPEED), //here the speed you made in the beginning is used
       
            // rect(12, 12),
            area(),
            offscreen({ destroy: true }),
            anchor("center"),
            // color(BLUE),
            "bullet",
        ])


    }


    await wait(1)
    enemy.enterState("move")


})
```


This creates the bullet npc and when the player is still alive it will attack the player using the bullet and when the player is still alive it will wait `1` second and then move toward the player.


```js
enemy.onStateEnter("move", async () => {
    await wait(2)
    enemy.enterState("idle")
})


// Here we move towards the player every frame if the current state is "move"
enemy.onStateUpdate("move", () => {
    if (!player.exists()) return
    const dir = player.pos.sub(enemy.pos).unit()
    enemy.move(dir.scale(ENEMY_SPEED))
})
```


When an enemy moves, the enemy will move to the player's position if they are still alive and it will enter an `idle` state after waiting.




```js
player.onCollide("bullet", (bullet) => {
    destroy(bullet)
    destroy(player)
    addKaboom(bullet.pos)
})
```


this will make a working bullet

## summary
and with all that I create a game in [kaboom3.0.html](../kaboom3.0.html)

