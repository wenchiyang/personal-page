---
layout: page
title: Lifted Model Checking for Relational MDPs
description: An efficient formal verification framework for Safety in AI
---

[<a href="https://link.springer.com/article/10.1007/s10994-021-06102-7" target="_blank">Paper</a>]
[<a href="https://github.com/wenchiyang/pCTL-REBEL" target="_blank">Code</a>]

Model checking has been developed for verifying the behavior of systems with stochastic and non-deterministic behavior. It is used to provide guarantees
about such systems, an approach which is also adapted in safe reinforcement learning. While most model checking methods focus on propositional models properties, various probabilistic planning and reinforcement frameworks deal with relational domains, for instance, STRIPS planning and relational Markov Decision Processes.
Using propositional model checking in relational settings requires one to ground  the model, which leads to the well known state explosion problem and intractability.  

We present pCTL-REBEL, a lifted model checking approach for verifying pCTL properties on relational MDPs. It extends REBEL, the relational Bellman update operator, which is a lifted value iteration approach  
for  model-based relational reinforcement learning, toward relational model-checking. In this way, it contributes an enabling step towards safe model-based relational reinforcement learning.
pCTL-REBEL is lifted, which means that rather than grounding, the model exploits symmetries and reasons at an abstract relational level.

Theoretically, we show that the pCTL model checking approach
is decidable for relational MDPs even for possibly infinite domain  provided that the states have a bounded size.
Practically, we contribute algorithms  and an implementation of lifted relational model checking, which allows to show show that the lifted approach improves the scalability of the model checking approach.

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
</div> -->

<!-- <div class="img_row">
    <img class="col three left" src="{{ site.baseurl }}/assets/img/path.png" alt="logical regression" title="example image"/>
</div>
<div class="col three caption">
    Logical regression. The blocks started with an upper case letter is an abstract block, which can represent an arbitrary block.
</div> -->

<!-- You can also put regular text between your rows of images. Say you wanted to write a little bit about your project before you posted the rest of the images. You describe how you toiled, sweated, *bled* for your project, and then.... you reveal it's glory in the next row of images. -->


<!-- <div class="img_row">
    <img class="col two left" src="{{ site.baseurl }}/assets/img/2.jpg" alt="" title="example image"/>
    <img class="col one left" src="{{ site.baseurl }}/assets/img/3.jpg" alt="" title="example image"/>
</div>
<div class="col three caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>
 -->
<br/><br/>
