![Crowded city](city-1265055_1280.jpg)

In the quest for efficiency and effectiveness in urban transportation, finding the optimal routes to take passengers from their initial locations to their desired destinations is paramount. This challenge is not just about reducing travel time; it's about enhancing the overall experience for both drivers and passengers, ensuring safety, and minimizing environmental impact. 

You have been asked to revolutionize the way taxis navigate the urban landscape, ensuring passengers reach their destinations swiftly, safely, and satisfactorily. As an initial step, your goal is to build a reinforcement learning agent that solves this problem within a simulated environment.

## The Taxi-v3 environment
The Taxi-v3 environment is a strategic simulation, offering a grid-based arena where a taxi navigates to address daily challenges akin to those faced by a taxi driver. This environment is defined by a 5x5 grid where the taxi's mission involves picking up a passenger from one of four specific locations (marked as Red, Green, Yellow, and Blue) and dropping them off at another designated spot. The goal is to accomplish this with minimal time on the road to maximize rewards, emphasizing the need for route optimization and efficient decision-making for passenger pickup and dropoff.

### Key Components:
- **Action Space:** Comprises six actions where 0 moves the taxi south, 1 north, 2 east, 3 west, 4 picks up a passenger, and 5 drops off a passenger.
- **Observation Space:** Comprises 500 discrete states, accounting for 25 taxi positions, 5 potential passenger locations, and 4 destinations. 
- **Rewards System:** Includes a penalty of -1 for each step taken without other rewards, +20 for successful passenger delivery, and -10 for illegal pickup or dropoff actions. Actions resulting in no operation, like hitting a wall, also incur a time step penalty.

![Taxi-v3 environment snapshot](Taxi_snap.png)



```python
# Re-run this cell to install and import the necessary libraries and load the required variables
import numpy as np
import gymnasium as gym
import imageio
from IPython.display import Image
from gymnasium.utils import seeding

# Initialize the Taxi-v3 environment
env = gym.make("Taxi-v3", render_mode='rgb_array')

# Seed the environment for reproducibility
env.np_random, _ = seeding.np_random(42)
env.action_space.seed(42)
np.random.seed(42)

# Maximum number of actions per training episode
max_actions = 100 
```


```python
# define/initialize
n_eps = 2000
n_actions = env.action_space.n
n_states = env.observation_space.n

q_table = np.zeros((n_states, n_actions))
episode_returns = []

epsilon = 0.1
alpha = 0.1
gamma = 0.9
epsilon_factor = 0.995
min_epsilon = 0.01

def epsilongreedy(state): 
    if np.random.rand() < epsilon: 
        action = env.action_space.sample() 
    else:
        action = np.argmax(q_table[state, :])
    return action
    
def update_q_table(state, action, reward, next_state): 
    next_best_action = np.argmax(q_table[next_state, :])
    q_table[state, action] = (1-alpha)*q_table[state, action]+alpha*(reward+gamma*q_table[next_state, next_best_action])
    
episode_returns = []

```


```python
# interaction loop
for episode in range(n_eps): 
    state, info = env.reset()
    terminated = False
    episode_reward = 0
    while not terminated: 
        action = epsilongreedy(state)
        next_state, reward, terminated, truncated, info = env.step(action)
        update_q_table(state, action, reward, next_state)
        episode_reward += reward
        state = next_state
    episode_returns.append(episode_reward)
    epsilon = max(min_epsilon, epsilon * epsilon_factor)
```


```python
# make policy
policy = {}
for state in range(n_states): 
    policy[state] = np.argmax(q_table[state, :])
```


```python
# test policy: 1 episode

np.random.seed(42)

state, info = env.reset(seed = 42)
terminated = False
episode_total_reward = 0
steps=0
max_steps = 16
frames = []

frames.append(env.render())

while not terminated and steps < max_steps:
    action = policy[state]
    next_state, reward, terminated, truncated, info = env.step(action)

    frames.append(env.render())
    episode_total_reward += reward
    state = next_state
    steps += 1
    
if episode_total_reward >= 4: 
    print(f"learning was successful. total reward: {episode_total_reward}")
else: 
    print(f"learning unsuccessful. total reward: {episode_total_reward}")

```

    learning was successful. total reward: 8



```python
# Once you are done, run this cell to visualize the agent's behavior through the episode
# Save frames as a GIF
imageio.mimsave('taxi_agent_behavior.gif', frames, fps=5, loop=0)

# Display GIF
gif_path = "taxi_agent_behavior.gif" 
Image(gif_path)
```




    <IPython.core.display.Image object>


