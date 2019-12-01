[//]: # (Image References)

[image1]: https://user-images.githubusercontent.com/10624937/42135623-e770e354-7d12-11e8-998d-29fc74429ca2.gif "Trained Agent"
[image2]: ./images/Episodes.png "Episodes"
[image3]: ./images/Plot.png "Plot"
[image4]: ./images/Hyper_F.png "Hyper_F"
[image5]: ./images/Hyper_Tests.png "Hyper_Tests"


# Project 3: Collaboration and Competition
![Arm][image1]

## Glossary
This glossary may help the reader to understand the concepts used in this implementation. I collected these concepts in [Unity Tecnologies - Github](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Glossary.md) and in the [Deep Reinforcement Learning Nanodegree Program](https://www.udacity.com/course/deep-reinforcement-learning-nanodegree--nd893).

- **Action:** The carrying-out of a decision on the part of an agent within the environment.  
- **Agent:** Component which produces observations and takes actions in the environment. Agents actions are determined by decisions produced by a Policy.  
- **Decision:** The specification produced by a Policy for an action to be carried out given an observation.  
- **Environment:** The scene which contains Agents and the Academy.  
- **Observation:** Partial information describing the state of the environment available to a given agent. (e.g. Vector, Visual, Text)  
- **Policy:** Function for producing decisions from observations.  
- **Reward:** Signal provided at every step used to indicate desirability of an agent’s action within the current state of the environment.  
- **State:** The underlying properties of the environment (including all agents within it) at a given time.  


## Implementation  

Analyzing this environment, I realized I could solve this challenge by implementing an agent according to the DDPG paper. Both observation and action spaces are continuous, which is suitable for the actor-critic architecture proposed in that paper. Moreover, this environment is composed of two agents performing similar actions, given similar observations. Thus, the solution I adopted was:

One actor-network to control both agents: the Actor is used to approximate the optimal policy π deterministically. Since agents in this environment have the same goal and perform similar actions given similar observations, it is straightforward the use of one network for both agents in order to interpret the observation and take action.

One centralized critic-network to map the environment: the Critic calculates the optimal action-value function Q(s, a). As the observation and action spaces are similar, it is reasonable having only one critic to determine the value of the expected reward given those inputs.

The application of this solution (1 actor + 1 critic) is possible only because the agents perform actions in the same action-space, receive similar observations, and have the same goal. If they had conflicting goals, it would be necessary to have distinct critics. If they performed different actions, it would be required one actor-critic pair for each agent.
  
Below I describe model architecture, learning algorithm, and hyperparameters.  


### Model architecture  

The **Actor Network** receives as input 24^1 variables representing the observation space and generates as output 2 numbers representing the predicted best action for that observed state. That means, the Actor is used as the function approximator to the optimal policy π deterministically.

The **Critic Network** receives as input 24^1 variables representing the observation space. The result of the Critic's first hidden layer, after being normalized, is stacked with the action proceeding from the Actor Network to be passed in as input for the Critic's second hidden layer.
The output of this network is the predict target value, based on the given state and the estimated best action.
That means, the Critic is used as the function approximator to the optimal action-value function Q(s, a).

^1 Why 24 and not 8 variables as input?  
Although the observation space consists of 8 variables, the networks receive as input 3 Stacked Vectors summing up 24 variables.  
In cases where Vector Observations need to be remembered or compared over time, increasing the Stacked Vectors values allows the agent to keep track of multiple observations into the past.

  
Initially, I created the Actor and Critic networks each of them with two fully-connected hidden layers with ReLU activation and one fully-connected linear output layer. I defined my network as having an input of 24 variables, 400 nodes for the first hidden layer, 300 nodes for the second one.  

- fc_layers for the actor network: FC1: 400 nodes, FC2: 300 nodes
- fc_layers for the critic network: FC1: 400 nodes, FC2: 300 nodes  


### Learning algorithm
  
The training function receives from the environment states (_S_) corresponding to each one of the agents.  Those states are passed into the actor-local, which calculates the best action for them. Then, a noise process is applied to actions (_A_) in order to improve the exploration of the environment.  
In the next step, the actions are sent and performed in the environment, which returns a reward (_R_) and a next state (_S'_). The set of variables (_S, A, R, S'_) of each agent is an experience that is stored in a single replay buffer. When the replay buffer has sufficient experiences, a minibatch-sample is randomly selected to train the networks. For each experience in the minibatch, a target-reward is calculated.  
The target-reward is the sum of the reward received and the discounted-reward of the next states. The discounted-reward is calculated by the critic-target based on the next states and the next actions calculated by the actor-target. The target-reward is compared to the expected-reward, which is predicted by the critic-local, to find the mean squared error loss. This loss is used to update the critic-local weights.  
After being updated, the critic-local determines the state-based rewards along with the actions calculated by the actor-local.
The average of these rewards is used to update the actor-local weights. Following the update of the local networks, a soft update is performed to transfer the weights from the local to the target networks gradually.


### Hyperparameters
My next step was a tuning phase on the hyperparameters. Within this phase, I also revisited my implementations to verify if I had written the code correctly.  
It's a difficult task since a minimal change in one hyperparameter can lead to a significant change in the final result. 

- For the model I followed three tips given by Udacity:  

	- Keep Gamma high  
	- Keep learning rate low
	
- I used the **average score** to decide the better parameter  
 
For all tests I ran the project in GPU. See the results below:

![Hyper_Tests][image5]  


I achieved a good average score in the run **_(run 2)_** and I used the hyperparameters below:  

![Hyper_F][image4]  

## Plot of Rewards  
This graph shows the rewards per episode for the agents within the training phase, as well as the moving average.
It illustrates that the agents were able to receive an average score of +0.5 (over 100 consecutive episodes, after taking the maximum over both agents).  
The environment is considered solved, when the average (over 100 episodes) of those scores is at least +0.5.  

![Episodes][image2]  

![Plot][image3]

## Conclusion  

To solve this challenge, I studied and implemented the architecture proposed in the [DDPG paper](https://arxiv.org/abs/1509.02971).  Following an extensive fine-tuning phase, I reached a impressive result, solving the environment in only **274** episodes.
Working on projects like this one involves a great deal of reading, studying,  attention,  and patience, especially in the tailoring of the hyperparameters.


## Ideas for Future Work  
- There are other algorithms proposed to solve this kind of environment. One future work could implement them to verify their performance in this environment. Those algorithms are:

	- **MADDPG:** [Multi Agent Actor Critic for Mixed Cooperative Competitive environments](https://papers.nips.cc/paper/7217-multi-agent-actor-critic-for-mixed-cooperative-competitive-environments.pdf) by OpenAI
	- **TRPO:** [Trust Region Policy Optimization](https://arxiv.org/abs/1502.05477)
	- **GAE:** [Generalized Advantage Estimation](https://arxiv.org/abs/1506.02438)
	- **A3C:** [Asynchronous Advantage Actor-Critic](https://arxiv.org/abs/1602.01783)
	- **A2C:** Advantage Actor-Critic
	- **ACER:** [Actor Critic with Experience Replay](https://arxiv.org/abs/1611.01224)
	- **PPO:** [Proximal Policy Optimization](https://arxiv.org/pdf/1707.06347.pdf)
	- **D4PG:** [Distributed Distributional Deterministic Policy Gradients](https://arxiv.org/pdf/1804.08617.pdf)
	
- There more to learn about the [parameter-noise](https://openai.com/blog/better-exploration-with-parameter-noise/), which is really effective and help in better exploration.	
- I want to deep in the [Deep Reinforcement Learning for Self Driving Car](https://www.youtube.com/watch?v=MQ6pP65o7OM&feature=youtu.be) by MIT to know more about reinforcement learning algorithms and after to implement them in this project.  
- I am interested in learning to build my own Unity environments following the instructions [here](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Getting-Started-with-Balance-Ball.md), which walk you through all of the details of building an environment from a Unity scene.
