## Super power-ups!

Now that you have a new power-up collectable working, it’s time to make it do something really cool! Let's make it 'rain' power-ups for a few seconds, instead of just giving out an extra life.
 
For this, you're going to use another `broadcast`{:class="blockevents"} message.

+ First, change the `react-to-player`{:class="blockmoreblocks"} block to broadcast a message when the player character touches a type `2` collectable. Call the message `collectable-rain`{:class="blockevents"}.

```blocks
    define react-to-player (type)
    if <(type) = [1]> then
        change [points v] by (collectable-value)
    end
    if <(type) = [2]> then
        broadcast [collectable-rain v]
    end
```
 
Now you need to create a new piece of code inside the **Collectable** sprite scripts that will start whenever the `collectable-rain`{:class="blockevents"} message is broadcast.

+ Add this code for the **Collectable** sprite to make it listen out for the `collectable-rain`{:class="blockevents"} broadcast.

```blocks
    when I receive [collectable-rain v]
    set [collectable-frequency v] to [0.000001]
    wait (1) secs
    set [collectable-frequency v] to [1]
```

--- collapse ---
---
title: What does the new code do?
---

This piece of code waits to receive a broadcast, and responds by setting the `collectable-frequency`{:class="blockdata"} variable to a very small number, then waiting for one second, and then changing the variable back to `1`.

Let's look at how the `collectable-frequency`{:class="blockdata"} variable is used to find out why this makes it rain collectables.

In the main game loop, the part of the code that makes **Collectable** sprite clones gets told by the `collectable-frequency`{:class="blockdata"} variable how long to wait between making one clone and the next:

```blocks
    repeat until <not <(create-collectables) = [true]>>
        if < [50] = (pick random (1) to (50))> then
            set [collectable-type v] to [2]
        else
            set [collectable-type v] to [1]
        end
        wait (collectable-frequency) secs
        go to x: (pick random (-240) to (240)) y:(179)
        create clone of [myself v]
    end
```

You can see that the `wait` block here pauses the code for the length of time set by `collectable-frequency`{:class="blockdata"}. 

If the value of `collectable-frequency`{:class="blockdata"} is `0.000001`, the `wait` block only pauses for **one millionth** of a second, meaning that the `repeat until`{:class="blockcontrol"} loop will run many more times than normal. As a result, the code is going to create **a lot** more power-ups than it normally would, until `collectable-frequency`{:class="blockdata"} is changed back `1`.

Can you think of any problems that might cause? There’ll be a lot more power-ups…what if you kept catching them?

--- /collapse ---

### Challenge: get creative!
 
+ Based on this card and the previous one, you can now make as many different power-up collectables as you want! What about one that gives out 20 times the usual number of points, or adds three lives, or makes it so the player can’t run out of lives for a period of time? Come up with some cool power-ups and see if you can make them!
