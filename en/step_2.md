## Setting things up

Because you’re learning how to code in Scratch and not how to build a physics engine (code that makes things in a computer game behave like real-world objects, e.g. they don't fall through floors), you’ll be starting with a project I’ve created that already has the basics for moving, jumping, and detecting platforms built in.

You should take a quick look at the project, including the details on this card, because you’ll be making some changes to it later, but you don’t need to understand everything it’s doing!

### Get the project

+ The first thing you’ll need to do is to get a copy of the Scratch code from [dojo.soy/advanced-scratch2](http://dojo.soy/advanced-scratch2){:target="_blank"} 

### Take a look at the code

The physics engine of the game has a variety of pieces in it, some of which work already and some of which don’t yet. You can test this out by running the game and trying to play it.

You'll see that you can lose lives, but nothing happens when you run out. Also, the game only has one level, one type of thing to collect, and no enemies. You’re going to fix all of that, and a bit more!

+ For now, take a look at how the code is put together. It uses lots of **More** blocks, which are great for splitting your code up into pieces so you can manage it better. A **More** block is a block you make up out of a lot of other blocks, and you can give some instructions to it. You'll see how it works in an upcoming Card!

![](images/setup2and3.png)

### 'More' blocks are really useful

In the code above, the main game `forever`{:class="blockcontrol"} loop calls the `main-physics`{:class="blockmoreblocks"} **More** block to do a whole lot of stuff! Keeping the blocks separated like this makes it easy to read the main loop and understand what happens in the game, without worrying about **how** it happens.
 
+ Now look at the `reset game`{:class="blockmoreblocks"} and `reset character`{:class="blockmoreblocks"} **More** blocks:
1. They do pretty normal things, such as setting up variables and making sure the character rotates properly
1. `reset-game`{:class="blockmoreblocks"} **calls** `reset-character`{:class="blockmoreblocks"}, showing you that you can use a **More** block inside another **More** block
1. The `reset-character`{:class="blockmoreblocks"} **More** block gets used in two different places in the main loop. This  means you can change two places in your main game loop by only changing the code inside of the **More** block, which saves you a lot of work and helps you avoid mistakes.
