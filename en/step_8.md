## Moving platforms

The reason I asked you to use my version of level 2 is the gap you might have noticed in the middle of the layout. You’re going to create a platform that moves through this gap and that the player can jump on and ride!

![Another level with different platforms](images/movingPlatforms.png)

+ First, you’ll need the sprite for the platform: add a new sprite, name it **Moving-Platform**, and using the costume customisation tools in the Costumes tab to make it look like the other platforms \(use vector mode\).

Now let's adde some code to the sprite! 

+ Begin with the basics: to make a never-ending set of platforms moving up the screen, you’ll need to clone the platform at regular intervals. I picked `4` seconds as my interval. You also need to make sure that there’s an on/off switch for making the platforms, so that they don’t show up in level 1. I’m using a new variable called `create-platforms`{:class="blockdata"}. 

Here's how my code looks so far for the new sprite:

```blocks
    when green flag clicked
    hide
    forever
        wait (4) secs
        if <(create-platforms) = [true]> then
            create clone of [myself v]
        end
    end
```

+ Then add the clone's code:

```blocks
    when I start as a clone
    show
    forever
        if <(y position) < [180]> then
            change y by (1)
            wait (0.02) secs
        else
            delete this clone
        end
    end
```

This code is simple: it makes the **Moving-Platform** clone move up to the top of the screen, slowly enough for the player to jump on and off, and then disappear. 

+ You need to make the platforms disappear/reappear based on the broadcasts that change levels, and the `game over` message. 

```blocks
    when I receive [level-1 v]
    set [create-platforms v] to [false]
    hide

    when I receive [level-2 v]
    set [create-platforms v] to [true]

    when I receive [game over v]
    hide
    set [create-platforms v] to [false]
```

+ Now, if you try to actually play the game, the **Player Character** falls through the platform! Any idea why? 

It’s because the physics code doesn’t know about the platform. It’s actually an easy fix: inn the **Player Character** sprite scripts, replace every `touching “Platforms”`{:class="blocksensing"}  block with an `OR`{:class="blockoperators"} operator that checks for **either** `touching “Platforms”`{:class="blocksensing"}  **OR** `touching “Moving-Platform”`{:class="blocksensing"}.
 
+ Go through the code for the **Player Character** sprite and everywhere you see this block:

```blocks
    <touching [Platforms v] ?>
```

replace it with this one:

```blocks
    <<touching [Platforms v] ?> or <touching [Moving-Platform v] ?>>
```
