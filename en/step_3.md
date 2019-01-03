## Losing the game

First things first! You need a way to make the game end when the player has run out of lives. At the moment that doesn't happen.

You may have noticed that the `lose`{:class="blockmoreblocks"} **More** block in the scripts for the **Player Character** sprite is empty. You’re going to fill this in and set up all the pieces needed for a nice 'Game over' screen.

+ First, find the `lose`{:class="blockmoreblocks"} block and complete it with the following code: 

```blocks
    define lose
    stop [other scripts in sprite v] :: control stack
    broadcast [game over v]
    go to x:(0) y:(0)
    say [Game over!] for (2) secs
    stop [all v]
```

--- collapse ---
---
title: What does this code do?
---

Whenever the `lose`{:class="blockmoreblocks"} block runs now, what it does is: 

1. Stop the physics and other game scripts acting on the **Player Character**
1. Tell all the other sprites that the game is over by **broadcasting** a `game over` message they can respond to and change what they're doing
1. Move the **Player Character** to the centre of the Stage and have them tell the player that the game is over
1. Stop all scripts in the game

--- /collapse ---

Now you need to make sure all the sprites know what to do when the game is over, and how to reset themselves when the player starts a new game. **Don’t forget that any new sprites you add also might need code for this!**

### Hiding the platforms and edges

+ Start with the easiest sprites ones. The **Platforms** and **Edges** sprites both need code for appearing when the game starts and disappearing when they receive the `game over` broadcast, so add these blocks to each of them:

```blocks
    when I receive [game over  v]
    hide
```

```blocks
    when green flag clicked
    show
```

### Stopping the stars

Now, if you look at the code for the **Collectable** sprite, you’ll see it works by **cloning** itself. That is, it makes copies of itself that follow the special `when I start as a clone`{:class="blockevents"} instructions. 

We’ll talk more about what makes clones special when we get to the Card about making new and different collectables. For now, what you need to know is that clones can do **almost** everything a normal sprite can, including receiving `broadcast`{:class="blockevents"} messages.

+ Let’s look at how the **Collectable** sprite works. See if you can understand some of its code: 

```blocks
    when green flag clicked
    hide
    set [collectable-value v] to [1]
    set [collectable-speed v] to [1]
    set [collectable-frequency v] to [1]
    set [create-collectables v] to [true]
    set [collectable-type v] to [1]
    repeat until <not <(create-collectables) = [true]>>
        wait (collectable-frequency) secs
        go to x: (pick random (-240) to (240)) y: (179)
        create clone of [myself v]
    end
```

1. First it makes the original **Collectable** sprite invisible by hiding it
1. Then it sets up the control variables — we’ll come back to these later
1. The `create-collectables`{:class="blockdata"} variable is the on/off switch for cloning: the loop creates clones if `create-collectables`{:class="blockdata"} is `true`, and does nothing if it’s not

+ Now you need to set up a block for the **Collectable**  sprite so that it reacts to the `game over` broadcast:

```blocks
    when I receive [game over v]
    hide
    set [create-collectables v] to [false]
```

This code is similar to the code controlling the **Platforms** and **Edges** sprites. The only difference is that you’re also setting the `create-collectables`{:class="blockdata"} variable to `false` so that no new clones get created when it's 'Game over'. 
 
+ Note that you can use the `create-collectables`{:class="blockdata"} variable to pass messages from one part of your code to another! 
