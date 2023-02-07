---
layout: page
title: Safe Reinforcement Learning vis Probabilistic Logic Shield 
description: A deep reinforcement learning framework that ensures safety of the learning agent by applying logical constraints on the neural policy.
img: /assets/img/blocksworld.png
---

Some figures in this blog post are generated using
<a href="https://openai.com/dall-e-2/" target="_blank">DALL.E</a>.



<center>
<img src="{{ site.baseurl }}/assets/img/ellenselfdrivingcar.png" alt="Ellen and a self driving car" width="400">
</center>


>Ellen had heard a lot about the new self-driving cars, and while the idea of being 
able to sit back and relax while her car took her where she needed to go sounded 
appealing, she wasn't sure if she could trust the technology. She made her way to 
the nearest self-driving car service station and climbed into the back seat of the 
sleek, futuristic vehicle. The car pulled away from the curb, merged into 
traffic and navigated the busy city streets with ease, smoothly weaving in and out of traffic and coming to a stop at each red 
light without hesitation.
> 
>As the car approached a busy intersection, Ellen heard the sound of sirens 
in the distance. An ambulance came into view, speeding down the road with its lights 
flashing. Ellen's self-driving car hesitated for a moment, and then made the decision 
to run the red light and pull over to the side of the road, making way for the 
ambulance. Ellen was impressed by the car's quick thinking, and felt a newfound 
sense of trust in the technology. As the ambulance passed by, she could see the 
paramedics inside, working to save the life of the patient. She realized that the 
self-driving car had made the right decision, prioritizing the safety of others even
at the risk of breaking the law.
> 
>Ellen began to relax, even enjoying the ride. All of a sudden, a screech of tires and
the sickening sound of metal colliding with metal pierced the air. In an instant, 
Ellen's car had been T-boned by another vehicle. She was thrown violently to the 
side, and when she looked up, she saw that the other car was a self-driving vehicle 
as well. It had apparently failed to stop at a red light and had plowed into Ellen's 
car.


The self-driving car scenario demonstrates the need for a machine learning system 
that can reason about complex, dynamic environments and make decisions based on 
uncertain and incomplete information. 
In such a scenario, the system must be able to understand and navigate a world 
that consists of many types of objects such as cars,
traffic lights, intersections and people that can move over time. 
The system must also be able to handle heterogeneous data from multiple sources, 
such as LiDAR, real-time traffic information and geographical maps. 
The system must be able to combine short-term actions and long-term planning. 
For example, the self-driving car 
should follow a route to a destination within a reasonable period of time, taking 
into account potential obstacles on the way.

Most importantly, intelligent systems should prioritize safety in order 
to earn public trust. If an ambulance with sirens and flashing lights is approaching,
the system should be able to infer that it is likely on a mission and
that in some situations, it would be safe to run a red light in order
to make way for the ambulance. 

Intelligent systems can, and should, provide quantitative 
metrics and evaluations so that safety can be demonstrated and improved.

## Safety via a shield


Shielding is a popular Safe Reinforcement Learning (Safe RL) technique that 
aims at finding an optimal policy while staying safe.
To do so, it relies on a **shield**, a logical component that monitors the 
agent's actions and rejects those that violate the given safety constraint.
While early shielding techniques operate completely on symbolic state spaces, 
more recent ones have incorporated a neural policy learner to handle continuous 
state spaces. In this blog, we will also focus 
on **integrating shielding with neural policy learners**.

<center>
<img src="{{ site.baseurl }}/assets/img/donotturnright.png" alt="" width="500">
<div class="col three caption">
A self-driving car encounters a red light and another vehicle in front to its right.
In this scenario, an *intelligent* car probably should not accelerate or turn right.
</div>
</center>

TODO

