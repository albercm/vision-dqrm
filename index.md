# Disentangled Planning and Control in Vision Based Robotics via Reward Machines
In this work we augment a Deep Q-Learning agent with a Reward Machine (DQRM) to increase speed of learning vision-based policies for robot tasks, and overcome some of the limitations of DQN that prevent it from converging to good-quality policies. A reward machine (RM) is a finite state machine that decomposes a task into a discrete planning graph and equips the agent with a reward function to guide it toward task completion. The reward machine can be used for both reward shaping, and informing the policy what abstract state it is currently at. An abstract state is a high level simplification of the current state, defined in terms of task relevant features. These two supervisory signals of reward shaping and knowledge of current abstract state coming from the reward machine complement each other and can both be used to improve policy performance as demonstrated on several vision based robotic pick and place tasks. Particularly for vision based robotics applications, it is often easier to build a reward machine than to try and get a policy to learn the task without this structure.


## Take Home Message

#### We are concerned with...
... addressing the **limitations** of RL agents: 

- DQN may not learn good-quality policies, even in very simple tasks
- Stronger supervision signals are needed

#### To this end, we...
... augment DQN agents with reward machines (DQRM), which provide **strong supervision** signals via:

- Principled **denser reward** functions
- Augmented observations, with **abstract states**

#### And we show that...
... for vision-based robotic manipulation tasks, DQRM greatly outperforms vanilla DQN and:
- Helps **disentangle** planning and control
- Is more **sample-efficient**
- Learns **better-quality** policies

- - - -

## Constructing Reward Machines from Demonstrations

### Thinking in Abstract
Humans solve complex tasks by planning and reasoning in a high-level degree of **abstraction**. Usually, we only get into the low-level details when we need to solve specific subtasks. Based on this idea, we augment DQN with an abstract planning graph that divides tasks into subtasks.
- The abstracted graph structure is used to decide on **high level planning** actions.
- The transitions between abstract state involve **low-level control** actions.

### Reward Machines from Demonstrations
Reward machines are graph structures that drive RL agents along different modes of operation, and issue rewards that incentive task completion [Toro Icarte et al., 2018]. 

We construct reward machines from demonstrations:
- Abstract states are constructed with feature detectors.
- Abstract planning graphs are constructed by composing abstract state transitions.
- Reward machines are constructed by equipping the graph with a principled reward function [Camacho et al., 2017, Camacho et al., 2019].

## DQN Extended with Reward Machines (DQRM)
Reward machines extend DQN with two extra supervision signals that improve their learning performance:

1. **An abstract state ID**
- DQN has to learn fewer latent features.
- Policies are conditioned on fewer latent variables, and therefore are made easier to learn.

2. **A denser reward structure**
- Denser rewards guide search more effectively than with sparse episodic rewards.

## Experimental results
The supervision signals in DQRM make it easier to learn policies that perform the right high-level planning action (pick or place) and also the right low-level control action (target coordinates of the gripper). 

DQRM learns **better-quality** policies with **fewer training steps**, compared to baseline DQN and other ablations.
- DQN: the standard version of Deep Q Learning [Mnih et al., 2015], with a ResNet network, and trained with unit rewards in goal states.
- DQN(AS): DQN, with observations extended with abstract state IDS.
- DQN(RS): DQN, with denser rewards (reward shaping) coming from the reward machine.
- DQRM: DQN with reward machines. It combines the benefits of DQN(AS) and DQN(RS).

## Summary and Discussion
Reward machines provide two sources of supervision signal to RL agents: abstract states, and denser rewards. We showed that reward machines can be exploited to disentangle planning and control, and overcome some of the limitations of DQN in vision-based robotic tasks.

#### Limitations of DQN
In vision-based robotics tasks, DQN has to solve two subproblems in parallel: learning meaningful latent features, and learning a policy conditioned on such features. Whereas solving each of these two subproblems may be easy, we showed that solving the two tasks at once can be challenging even in simple pick and place tasks.

#### Benefits of Extending DQN with Reward Machines in Vision-based Robotics
DQRM can exploit the supervision signals from reward machines to learn better-quality policies in fewer training steps. This is because abstract states can help disambiguate planning from control, and policies are made easier to learn because they depend on fewer latent features.

- - - -

## References
[Toro Icarte et al., 2018] [Using Reward Machines for High-Level Task Specification and Decomposition in Reinforcement Learning.](http://proceedings.mlr.press/v80/icarte18a.html) Rodrigo Toro Icarte, Toryn Q. Klassen, Richard Valenzano, and Sheila A. McIlraith. In Proceedings of the 35th International Conference on Machine Learning (ICML), pages 2112-2121, 2018.

[Camacho et al., 2017] [Non-Markovian Rewards Expressed in LTL: Guiding Search Via Reward Shaping.](http://www.cs.toronto.edu/~sheila/publications/cam-etal-socs17.pdf) Alberto Camacho, Oscar Chen, Scott Sanner, and Sheila A. McIlraith. In Proceedings of the Tenth International Symposium on Combinatorial Search (SoCS), pages 159-160, 2017.

[Camacho et al., 2019] [LTL and Beyond: Formal Languages for Reward Function Specification in Reinforcement Learning.](https://www.ijcai.org/Proceedings/2019/840) Alberto Camacho, Rodrigo Toro Icarte, Toryn Q. Klassen, Richard Valenzano, and Sheila A. McIlraith. In Proceedings of the 28th International Joint Conference on Artificial Intelligence (IJCAI), pages 6065-6073, 2019.

[Mnih et al., 2015] [Human-level Control Through Deep Reinforcement Learning.](https://www.nature.com/articles/nature14236) Volodymyr Mnih, Koray Kavukcuoglu, David Silver, Andrei A. Rusu, Joel Veness, Marc G. Bellemare, Alex Graves, Martin A. Riedmiller, Andreas Fidjeland, Georg Ostrovski, Stig Petersen, Charles Beattie, Amir Sadik, Ioannis Antonoglou, Helen King, Dharshan Kumaran, Daan Wierstra, Shane Legg, Demis Hassabis. In Nature, 518(7540): 529-533, 2015.
