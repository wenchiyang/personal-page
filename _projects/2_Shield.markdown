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

## Driving safely using a shield: Why not?

Shielding is a popular Safe Reinforcement Learning (Safe RL) technique that 
aims at finding an optimal policy while staying safe.
To do so, it relies on a **shield**, a logical component that monitors the 
agent's actions and rejects those that violate the given safety constraint.

<center>
<img src="{{ site.baseurl }}/assets/img/shielding.png" alt="" width="600">
<div class="col three caption">
Shielding in the Reinforcement Learning loop. The <tt>agent</tt> consults the 
<tt>shield</tt> whether the action it wants to perform is safe or not. Only when
the action is accepted by the shield (i.e. a "yes" from the shield) does the agent 
perform the action in the environment.
</div>
</center>


While early shielding techniques operate completely on symbolic state spaces,
more recent ones have incorporated a neural policy learner to handle continuous
state spaces. In this blog, we will also focus
on **integrating shielding with neural policy learners**.

<center>
<img src="{{ site.baseurl }}/assets/img/donotturnright.png" alt="" width="500">
</center>

Consider a self-driving agent that encounters a red light and another vehicle in 
front to its right. The agent is implemented as a neural network that takes 
visual input and produces a policy, i.e. a probabilistic distribution over actions 
<tt>{do-nothing, accelerate, brake, turn-left, turn-right}</tt>. 
The agent samples actions from this policy until it finds one that is accepted 
by the shield. In this scenario, the shield determines that accelerating 
or turning right is unsafe and will only accept actions in
<tt>{do-nothing, brake, turn-left}</tt>. For example, the agent's policy could be:

<tt>
0.1::do-nothing, <br/>
0.5::accelerate, <br/>
0.1::brake, <br/>
0.1::turn-left, <br/>
0.2::turn-right <br/>
</tt>

The agent samples the first action <tt>accelerate</tt>, which is rejected by the shield.
The agent samples the second action <tt>accelerate</tt>, which is also rejected by the shield.
Then, the agent samples the third action <tt>turn-left</tt>, which is accepted by the shield. 
The agent controls the vehicle to <tt>turn-left</tt> and receives a reward of +5.

This seems okay at first glance, however, this framework is too simple to 
capture many aspects of the real world.


1. Not all actions are completely safe or unsafe. In fact, no actions are completely safe in driving scenarios.
2. Sensors can be noisy and not deterministic, for example, the sensor may detect an obstacle with a probability of 0.4.
3. The shield requires an accurate environment model, which is not always available, such as knowing road conditions, weather, and friction.
4. Traditional shielding techniques can be difficult to integrate with continuous, end-to-end deep reinforcement learning methods.
5. Even with perfect safety information in all states, rejection-based shielding may fail to learn an optimal policy. It may result in an extremely safe but non-rewarding policy that always stays put.

To mitigate the these issues, we must answer two questions:

1. How can the shield capture **uncertainties** in the real world? 
2. How should the interface between the agent and the shield be expanded to incorporate both *uncertainty* and *safety*?  

## More ideally, driving **safer** using a **probabilistic** shield

A more ideal learning process may look like this: Consider the same scenario,
but now the shield has probabilistic perception to detect the probability
of there being an obstacle. The first rule states "the probability of there being 
an obstacle in front is 0.8."

<tt>
0.8::obstacle(front). <br/>
0.2::obstacle(left). <br/>
0.5::obstacle(right) <br/>
</tt>

The shield incorporates the perception into its safety knowledge. 
The first rule is an if-else statement, stating that 
"if there is an obstacle in front, and the agent accelerates, then a crash 
will occur with a probability 0.9."

<tt>
0.9::crash:-obstacle(front), accelerate.
0.4::crash:-obstacle(left), turn-left.
0.4::crash:-obstacle(right), turn-right.
</tt>

By combining agent's policy, shield's perception and safety knowledge, we 
obtain a **safer policy**: 

<tt>
0.17::do-nothing, <br/>
0.24::accelerate, <br/>
0.17::brake, <br/>
0.15::turn-left, <br/>
0.27::turn-right <br/>
</tt>

Let's analyze, informally, why this policy is safer:

1. The probability of safer actions, i.e. <tt>do-nothing</tt>, <tt>brake</tt>, and <tt>turn-left</tt>, is higher.
2. The probability of <tt>accelerate</tt> is 50% lower given:
   - a high probability (0.8) of an obstacle in front
   - a high probability (0.9) of a crash if accelerating
3. The probability of <tt>turn-right</tt> is only slightly higher given:
   - a probability (0.5) of an obstacle on the right
   - a probability (0.4) of a crash if turning right

Using this safe policy, the agent will sample and perform an action in the 
environment. This shielding process provides a more realistic safety measure
by integrating safety and uncertainty.


## Status of project

We propose probabilistic shields as an alternative to the deterministic 
rejection-based shields.
Essentially, probabilistic shields take the original policy and noisy sensor 
readings to produce a safer policy, as demonstrated below.

<center>
<img src="{{ site.baseurl }}/assets/img/highlevel.png" alt="" width="500">
<div class="col three caption">
A comparison between a traditional rejection-based shield (<tt>SHLD</tt>) 
and a Probabilistic Logic Shield (<tt>PLS</tt>). 
**Left**: The policy must keep sampling actions until a safe action is 
accepted by the rejection-based shield. This requires an assumption that
an action is either completely safe or unsafe.
**Right**: We replace <tt>SHLD</tt> with <tt>PLS</tt> that proposes a safer 
policy without imposing the assumption.
</div>
</center>

By explicitly connecting action safety to probabilistic semantics,
probabilistic shields provide a realistic and principled way to 
balance return and safety. This also allows for shielding to be 
applied at the policy level instead of at the individual action level, 
which is typically done in the literature.

At the moment, this work is under review.