[//]: # (Image References)

[image1]: https://user-images.githubusercontent.com/10624937/42135623-e770e354-7d12-11e8-998d-29fc74429ca2.gif "Trained Agent"
[image2]: ./images/Navigate.png "Navigate"
[image3]: ./images/Environment.png "Environment"
[image4]: ./images/Run.png "Run"


# Project 3: Collaboration and Competition

## Introduction

In this project, I worked with the [Tennis](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Learning-Environment-Examples.md#tennis) environment.

![Trained Agent][image1]

In this environment, two agents control rackets to bounce a ball over a net. If an agent hits the ball over the net, it receives a reward of +0.1.  If an agent lets a ball hit the ground or hits the ball out of bounds, it receives a reward of -0.01.  Thus, the goal of each agent is to keep the ball in play.

The observation space consists of 8 variables corresponding to the position and velocity of the ball and racket. Each agent receives its own, local observation.  Two continuous actions are available, corresponding to movement toward (or away from) the net, and jumping. 

The task is episodic, and in order to solve the environment, your agents must get an average score of +0.5 (over 100 consecutive episodes, after taking the maximum over both agents). Specifically,

- After each episode, we add up the rewards that each agent received (without discounting), to get a score for each agent. This yields 2 (potentially different) scores. We then take the maximum of these 2 scores.
- This yields a single **score** for each episode.

The environment is considered solved, when the average (over 100 episodes) of those **scores** is at least +0.5.

## Getting Started
### Files included in this repository   

- The files below are needed to create and to train the two agents:
    - Tennis.ipynb 
    - ddpg_agent.py  
    - model.py 

- The trained model:
    - checkpoint.pth

- Useful information about the project:
    - README.md

- File describing the development process and the learning algorithm:
    - Report.md  
    
## Preparing the environment to run the project  

**1.** Install the Anaconda Platform

- For more information about it, see [Anaconda Installation Guide](https://docs.anaconda.com/anaconda/install/)
- Download the [Anaconda Installer](https://www.anaconda.com/distribution/), version Python 3.x

**2.** Create and activate a new environment for this project

- Open Anaconda Prompt (Windows) or Terminal (Linux) and type

```
conda create --name compet python=3.6 -y
```  

```
conda activate compet
```

**3.** Install Pytorch  
Numpy and other required libraries will be installed with Pytorch.

 - In your Anaconda Prompt (Windows) or Terminal (Linux) type the line command below

```
conda install pytorch -c pytorch -y
```
**4.** Install Unity Agents  
Matplotlib, Jupyter, TensorFlow, and other required libraries will be installed with Unity Agents.

 - In your Anaconda Prompt (Windows) or Terminal (Linux) type the line command below
 
```
pip install unityagents
```

## Getting the code
There are two options to get this project:

**1.** Download it as a zip file  

 - Do the download in this [link](https://github.com/LucianaRocha/drlnd-project-3-collab-compet.git).  
 - Unzip the file in a folder of your choice.

**2.** Clone this repository using Git version control system 
 
 - Open Anaconda Prompt (Windows) or Terminal (Linux), navigate to the folder of your choice, and type the command below:  

```
git clone https://github.com/LucianaRocha/drlnd-project-3-collab-compet.git
```

 - If you want to know more about Git, see this [link](https://git-scm.com/downloads).
 

## Download the environment 


Download the environment, unzip (or decompress) the file, and place it inside the folder drlnd-project-3-collab-compet-master, where you downloaded or cloned the repository. You need only select the environment that matches your operating system:  

- Linux: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Linux.zip)  
- Mac OSX: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis.app.zip)  
- Windows (32-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Windows_x86.zip)  
- Windows (64-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Windows_x86_64.zip)  
    
(_For Windows users_) Check out [this link](https://support.microsoft.com/en-us/help/827218/how-to-determine-whether-a-computer-is-running-a-32-bit-version-or-64) if you need help with determining if your computer is running a 32-bit version or 64-bit version of the Windows operating system.

(_For AWS_) If you'd like to train the agent on AWS (and have not [enabled a virtual screen](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Training-on-Amazon-Web-Service.md)), then please use [this link](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Linux_NoVis.zip) to obtain the "headless" version of the environment.  You will **not** be able to watch the agent without enabling a virtual screen, but you will be able to train the agent.  (_To watch the agent, you should follow the instructions to [enable a virtual screen](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Training-on-Amazon-Web-Service.md), and then download the environment for the **Linux** operating system above._)



## Running the project  
- Open Anaconda Prompt (Windows) or Terminal (Linux), navigate to the project folder, and type the commands below to activate the project environment, and to open Jupyter.  
If you are keen to know more about notebooks and other tools of Project Jupyter, you find more information on this [website](https://jupyter.org/index.html).  

```
conda activate compet 
```

```
jupyter notebook  
```

- Click on the Tennis.ipynb to open the notebook. 

![Navigate][image2]

- Next, we will start the environment! Before running the code cell below, change the file_name parameter to match the location of the Unity environment that you downloaded.

![Environment][image3]

- Run the notebook:

![run][image4]