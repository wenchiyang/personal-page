---
layout: page
title: Lifted Model Checking for Relational MDPs
description: An efficient formal verification framework for Safety in AI
img: /assets/img/blocksworld.png
---

[<a href="https://link.springer.com/article/10.1007/s10994-021-06102-7" target="_blank">Paper</a>]
[<a href="https://github.com/wenchiyang/pCTL-REBEL" target="_blank">Code</a>]

## Why model checking?

Probabilistic model checking aims at deciding whether a stochastic model satisfies a given probabilistic property. It provides rigorous guarantees about the model and allows for statements such as

- the machine gives a warning before shutting down with a probability higher than 0.95
- the emergency power supply, after giving a warning, continues to function for at least 10 more minutes with a probability higher than 0.9

We illustrate probabilistic model checking using a simple blocks world.
Imagine there are a number of blocks numbered with 1, 2, etc. 
Each block has one of the 4 colors: green, red, blue or yellow, e.g. a red block with ID 1 is expressed as <tt>red(1)</tt>. 
A block can be on another block, e.g. block 8 on block 1 is expressed as <tt>on(8,1)</tt>. 
A clear block (with no other block on top) is expressed as <tt>cl(1)</tt>. 
The following figure shows a configuration:

<center>
<tt>
red(1),    red(2),    green(3), green(4), green(5), <br/>
yellow(6), yellow(7), blue(8),  blue(9),  blue(10), <br/>
on(4,8),   on(8,1),   on(6,9),  on(5,7),  on(7,2), <br/>
cl(3),     cl(4),     cl(6),    cl(10),   cl(5) <br/>
</tt>
</center>

<br/>

<center>
<img src="{{ site.baseurl }}/assets/img/blocksworld.png" alt="A configuration" width="400">
<div class="col three caption">
    A configuration of 10 blocks.
</div>
</center>


**Question**: How many possible configurations are there if we have 10 blocks as shown in Fig 1?
**Answer**: Over 58 million configurations.

This is a surprisingly large number of configurations of only 10 blocks!
Actually, the number grows exponentially to the number of objects.
To see this, suppose there is only 1 red block, then we have exactly 1 configuration, 
i.e. red(1). If there are 2 red blocks, then we have 3 configurations,

<center>
<tt>
s1 = cl(1), cl(2)<br/>
s2 = cl(1), on(1, 2)<br/>
s3 = cl(2), on(2, 1)<br/>
</tt>
</center>

All these configurations are connected by **actions**. 
Say, we have a robot that can move around and arrange the blocks with a 
success probability of 0.9 (the robot fails to pick up a block with prob 0.1).

<center>
<img src="{{ site.baseurl }}/assets/img/abstracttransition.png" alt="A configuration" width="400">
<div class="col three caption">
    Moving a block succeeds with probability 0.9 and fails with probability 0.1. 
</div>
</center>



## Model Checking as a Human
The model checking problem is one step beyond counting the number of configurations and aims to find the configurations that satisfy a given safety constraint. A model checker can answer questions such as

**Property to be checked**:
What are the configurations that can reach a goal configuration in which a red block is on another red block within 5 actions with a probability larger than 0.8?

This requires checking  
1. Is the goal configuration reachable within 5 actions? 
2. If so, is the probability larger than 0.8?

If we have 10 blocks, then we must apply the transition 5 times for all 58 million states.
Enumerating all configurations alone already takes a long time. This is clearly not scalable, even with the state-of-the-art model checkers.

As a human being, we are great at abstracting the problem and reasoning at a high level.
The goal configuration is <tt>g = {on(X, Y), red(X), red(Y)}</tt>, 
meaning that *some* red block X is on *some other* red block Y.

It is trivial that <tt>{red(X), red(Y), cl(X), cl(Y)}</tt> is one action away 
from the goal by putting <tt>X</tt> on <tt>Y</tt>. 

Similarly, <tt>{red(X), red(Y), cl(Z), cl(Y), on(Z, X)}</tt>
is two actions away from the goal by moving <tt>Z</tt> away from <tt>X</tt>.
Additionally <tt>{red(X), red(Y), cl(Z), cl(Y), on(Z, Y)}</tt> is also two actions away 
from the goal by moving <tt>Z</tt> away from <tt>Y</tt>.

By following this reasoning, in order for the configuration in the above figure to satisfy the safety property,
We must 
1. use 4 actions to move away all 4 blocks away from block 1 and 2 
2. then, move block 1 on block 2 or block 2 on block 1
And all five actions must succeed and the robability is 0.9^5 = 0.59
Therefore, the configuration in the figure does not satisfy the constraint.

Voilà. We have solved the problem exactly without enumerating all 
58 million states by applying lifting. **Lifting** allows us to reason 
at the first-order level (i.e. in terms of X and Y but not actual blocks) 
and think about a group of configurations as a whole.
This is a core idea of **PCTL-REBEL**, the lifted model checker that we introduce in the paper.
I refer the readers to the paper for more mathematical details and the algorithms if you are wondering

- How exactly should the probability be computed, considering an action can fail?
- Can I check complex properties such as *what configurations can reach configuration A, and then configuration B*?
- What is the psduo-code of the algorithm?
- Is there a step-by-step demonstration of the algorith?
- ...


## Results

Based on this idea, we developed a lifted model checking algorithm PCTL-REBEL
We compare PCTL-REBEL with state-of-the-art model checkers PRISM and STORM. With a 30-minute time constraint, PCTL-REBEL can check 15 blocks, STORM can check 9 blocks and PRISM can check 8 blocks.

<center>
<img src="{{ site.baseurl }}/assets/img/PRISM_STORM_pCTLREBEL.png" alt="" width="400">
</center>

## Model Checking for infinite MDPs

When we have an unlimited number of blocks, the task of model checking 
becomes very difficult due to its NP-hard and undecidable nature.

The model checking problem is **undecidable** because an infinite number 
of blocks creates an infinitely large configuration, making it impossible
to enumerate all configurations. It is **NP-hard** because it resembles 
the halting problem, where the system must halt when it finds a configuration
that satisfies a given property, otherwise it continues to run indefinitely.

We obtain the decidability of the model checking problem for a class of 
infinite systems by creating a finite representation that contains all
necessary information about the underlying infinite system regarding 
a given property. This enables us to verify the system by examining the 
abstraction, which is equivalent to checking the infinite system. 
Furthermore, we have employed lifting techniques and have demonstrated 
that PCTL-REBEL can be utilized with minor modifications. 

The implementation is available online.


<!--
Every project has a beautiful feature shocase page. It's easy to include images, in a flexible 3-column grid format. Make your photos 1/3, 2/3, or full width.

To give your project a background in the portfolio page, just add the img tag to the front matter like so:

    ---
    layout: page
    title: Project
    description: a project with a background image
    img: /assets/img/12.jpg
    --- -->


<!-- <div class="img_row">
    <img class="col one left" src="{{ site.baseurl }}/assets/img/1.jpg" alt="" title="example image"/>
    <img class="col one left" src="{{ site.baseurl }}/assets/img/2.jpg" alt="" title="example image"/>
    <img class="col one left" src="{{ site.baseurl }}/assets/img/3.jpg" alt="" title="example image"/>
</div>
<div class="col three caption">
    Caption photos easily. On the left, a road goes through a tunnel. Middle, leaves artistically fall in a hipster photoshoot. Right, in another hipster photoshoot, a lumberjack grasps a handful of pine needles.
</div> 


<div class="img_row">
    <img class="col three left" src="{{ site.baseurl }}/assets/img/path.png" alt="logical regression" title="example image"/>
</div>
<div class="col three caption">
    Logical regression. The blocks started with an upper case letter is an abstract block, which can represent an arbitrary block.
</div> 
 -->

<!-- You can also put regular text between your rows of images. Say you wanted to write a little bit about your project before you posted the rest of the images. You describe how you toiled, sweated, *bled* for your project, and then.... you reveal it's glory in the next row of images. -->

<!-- 
<div class="img_row">
    <img class="col two left" src="{{ site.baseurl }}/assets/img/2.jpg" alt="" title="example image"/>
    <img class="col one left" src="{{ site.baseurl }}/assets/img/3.jpg" alt="" title="example image"/>
</div>
<div class="col three caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>
 -->

<br/><br/>
