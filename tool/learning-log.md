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

# 1/1/25

over the break I made a working boss fighting game where the play have to shoot to kill and move to dodged


### Sprites and Resources
 A list of object names (apple, coin, egg, etc.) is defined, and their sprites are loaded  from the image files. These images are used throughout the game to represent objects and boss.

 ```js 

 for (const obj of objs) { loadSprite(obj, `../img/${obj}.png`); }

 ```

 this is the code for using all the image in the file 

 ### Scenes: 
 The code shown two scenes, "battle" and "win". 
 
 The **"battle"** scene handles the main gameplay, like player movement, enemy spawning, shooting , and interactions like collisions and health of player and enemy. 
 
The **"win"** scene is triggered when the player defeats the boss, displaying the time taken and the boss defeated.


```js

scene("battle", () => { ... });
scene("win", ({ time, boss }) => { ... });

```

### Boss
boss and enemy are randomly spawned with different image, and the player have to avoid or destroy them. The boss has health, movement patterns, and collision detection. The health bar is going lower as the boss takes damage.


```js 

const boss = add([sprite(bossName), area(), pos(width() / 2, 40), health(BOSS_HEALTH), scale(3), "enemy"]);

```

### Bullet 
player can shoot bullets using the space bar. And the bullets are spawned at the player's position, and they move upwards, hitting enemies and dealing damage.

```js
onKeyPress("space", () => { spawnBullet(player.pos.sub(16, 0)); spawnBullet(player.pos.add(16, 0)); });
```

### grow(rate):

 This function scales an object over time, increasing its size gradually.

```js

function grow(rate) {
  return {
    update() {
      const n = rate * dt();
      this.scale.x += n;
      this.scale.y += n;
    },
  };
}
```
### late(t)
 This function delays the visibility of an object until after a specified amount of time , which is for showing elements after certain events.

```js
function late(t) {
  let timer = 0;
  return {
    add() {
      this.hidden = true;
    },
    update() {
      timer += dt();
      if (timer >= t) {
        this.hidden = false;
      }
    },
  };
}
```
### addExplode(p, n, rad, size)

 This function creates explosion effects at a specified position, creating small rectangles that grow and fade away.

```js
function addExplode(p, n, rad, size) {
  for (let i = 0; i < n; i++) {
    wait(rand(n * 0.1), () => {
      for (let i = 0; i < 2; i++) {
        add([pos(p.add(rand(vec2(-rad), vec2(rad))))]);
      }
    });
  }
}
```

## summory

this is a project that structure a game using scenes, sprites, and interactions while also managing game logic for player input, movement, and enemy behavior. 
[kaboom4.0.html](../kaboom4.0.html)


## 1/12/2025

## content

In this time I was tired so I learn a simple thing that will have sprite popping up every few second and disappearing

this is the code

```js

// Execute something after every 0.5 seconds.
loop(0.5, () => {

	const bean = add([
		sprite("apple"),
		pos(rand(vec2(0), vec2(width(), height()))),  //spawn the sprite in a random place
	])

	// Execute something after 3 seconds.
	wait(3, () => {
		destroy(bean)
	})

})

```

with this I made a simple program in [kaboom5.0.html](../kaboom5.0.html)

## 3/9/2025

## Content

This time I made a plan for my future plan on completing my project, in [Plan](../prep/plan.md), But I was sick over the break and couldn't do much, but right now my progress is I made a working map and player and enemy, but some problem I had was the bullet can't hit the enemy and the player can't jump. 

## Plan

I'm planning to make my player able to jump and bullet work and make the player base. In (kaboomfinal.html)[../kaboomfinal.html]

