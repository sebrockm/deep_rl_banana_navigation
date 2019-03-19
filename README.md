# deep_rl_banana_navigation

DRL Agent to collect Bananas in Unity environment


# Project: Navigation in a Banana World

### Intro

[//]: # (Image References)

[image1]: https://user-images.githubusercontent.com/10624937/42135619-d90f2f28-7d12-11e8-8823-82b970a54d7e.gif "Trained Agent"


This projects goal is to utilize Deep Reinforcement Learning (DRL) to train an agent to navigate through 2 dimensional environment and collect yellow bananas, while avoiding to collect blue bananas.

A trained agent will can be seen in below image: 

![Trained Agent][image1]

(source: https://github.com/udacity/deep-reinforcement-learning/blob/master/p1_navigation/README.md)

## Environment
### Action space
At each timestep the agent can choose between 4 (discrete) actions:

- **`0`** - move forward.
- **`1`** - move backward.
- **`2`** - turn left.
- **`3`** - turn right.

### State space

The state space is a continuous 37 dimensional vector that contains e.g. the agent's velocity and information about surrounding objects.

A reward of +1 is provided for collecting a yellow banana, and a reward of -1 is provided for collecting a blue banana. Hence, the goal of the agent is to collect as many yellow bananas as possible and to avoid blue bananas as well as possible.

The task is episodic. The problem is considered solved once the average score of the last 100 episodes is greater than 13.

## How to use

### Instructions

Follow the instructions in `Navigation.ipynb`. It provides links to the necessary installation precedure.
Then, execute the cells to train the agent.

### Expected Result
After approximately 700 episodes the agent reaches the goal of a score of 13 on average.
But it runs even further and reaches better results.

