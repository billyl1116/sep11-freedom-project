# Entry 2
##### 12/15/2024




## Context
In this entry I worked more on learning about Kaboom.js, and on the way of learning, I created some small projects that I used what I learned and created.




## 11/11/24








## Collision Detection




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




I tinker with all my code and created a working sprite that's movable and has collision and when It hit the image it sends a console.log message. In [kaboom.html](../kaboom.html)
















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
This makes it so the object will move from right to left and spawn objects at a certain place and when the player collides with the object they will lose.




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
this will make the npc enter `idle` for 0.5 second or you can change the time to whatever you want, and it will attack.




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




## EDP(Engineering Design Process)


I'm at the part where I'm a bit more than halfway done with my learning about my tools. And I'm hoping to be about to finish learning by the next entry.


## Skills


by learning about my tool, I was able to obtain that skill of how I can learn something easier and fast. And by trying out all the skills I think trying thing's out is the best way to learn and not just watch something happen. It's best to try it and get it into your brain.


## Future Plan


During the winter break I would be creating a small boss fighting game. Where the player has to shoot bullets and dogged objects coming at them to win the game. And by helping me learn I will watch this [video](https://www.youtube.com/watch?v=37rASpfnCCM&t=713) and go on [kaboom.js](https://kaboomjs.com/)to see how they made it.








[Previous](entry01.md) | [Next](entry03.md)


[Home](../README.md)

