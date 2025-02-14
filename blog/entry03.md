# Entry 3
##### 2/10/25


### Context

In this entry I worked more on learning about Kaboom.js, and on the way of learning, I created some small projects that I used what I learned and created, and after in the begining of making my project, I created a plan for my project.

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

## EDP(Engineering Design Process)


I'm at the part where I'm a bit more than halfway done with my learning about my tools. And I'm hoping to be about to finish learning by the next entry.



[Previous](entry02.md) | [Next](entry04.md)

[Home](../README.md)