# Report

## Algorithm
Hyperparameters and special algorithmic features are printed in **bold**.

This project uses a Deep Q-Network algorithm.
That means a neural network is used to approximate the Q-table.
For details on the neural network architecture see the section below.

The algorithm runs for at most **`n_episodes = 2000`** episodes.
In each episode the environment is reset and then actions are applied until the environment yields that it is done.
However, it runs for at most **`max_timesteps = 1000`** timesteps.
In each timestep an action is selected based on an **epsilon greedy** policy.
That means that with a probability of `epsilon` an action is randomly generated.
Otherwise the current neural network is fed with the current state and the resulting action is used.
This means, the current policy is applied.

At the beginning of each episode epsilon is intialized with `1.0`.
At the end of each timestep, epsilon is decayed by a factor of **`epsilon_decay = 0.995`**.
However, epsilon can never get smaller than **`epsilon_end = 0.01`**.
In other words, in the beginning of each episode, the actions are guaranteed to be generated.
With decreasing epsilon it becomes more and more likely that the current policy is used.

After an action is selected, it is applied in the environment and the corresponding award and new state are observed.
The corresponding `(state, action, reward, next_state, done)` tuple is enqueued into an **Experience Replay** buffer which has a maximum length of **`replay_buffer_size = 10000`**. 
The idea is to have a limited memory of past experiences that we can learn from again.
Since it's limited, the quality of the stored experiences will increase over time.

Only in every **`update_every = 5`**-th timestep the neural network comes into play:
A random sample of size **`batch_size = 64`** is drawn from the experience replay buffer.
The algorithm uses two identical networks, to apply the **Fixed Q-Targets** technique.
This technique decuples the input wheights from the wheight adjustment due to learning:
The "next states" (s') of the sample batch are fed into the secondary ("fixed") network.
Together with the reward and an applied discount rate of **`gamma = 0.995`**, this gives the target values.

The primary network is fed with the batch of current states.
This result and the target values are passed to the loss function (**medium quared error** is used in this project) to calculate an error.
This error is used to calculate the gradients needed to update the weights of the primary network. The learning rate was chosen to be **lr=0.001**.

Lastly, the wheights of the fixed network are actually not fixed, but are slightly updated with the new wheights of the primary network.
The factor of **`tau = 0.001`** denotes the portion of the new weights that are interpolated into the old ones.

### The neural network architecture
The network used in this solution has three layers:
  1. A recurrent layer (GRU) with hidden state size of 100
  2. A fully connected layer with 150 units
  3. An output layer with 4 units, which is the size of the action space
  
Between the layers 1 and 2 and between the layers 2 and 3 ReLU activation and dropout with a rate of 0.2 is applied.
The GRU layer can be fed with an arbitrary number of timesteps of states, its input shape is `(batch, rnn_seq_length, state)`.
In this project **`rnn_seq_length = 30`** was chosen, so 30 chronological states are combined to one state that is fed into the network.
By learning from a longer history, it is hopefully able to learn complicated moves like surrounding a blue banana.

### Training progress
![graph](basic_banana_scores.png?raw=true "basic_banana_scores")

## Output
Episode 100	Average Score: 0.53  
Episode 200	Average Score: 5.42  
Episode 300	Average Score: 8.99  
Episode 400	Average Score: 10.95  
Episode 490	Average Score: 12.97  
Environment solved in 491 episodes!	Average Score: 13.03  
Episode 500	Average Score: 13.12  
Episode 600	Average Score: 13.90  
Episode 700	Average Score: 13.47  
Episode 800	Average Score: 14.18  
Episode 900	Average Score: 13.50  
Episode 1000	Average Score: 14.10  
Episode 1026	Average Score: 14.93  

So, the goal is already reached after only ~500 episodes.

## Possible future Enhancements
In general, very many different combinations of the hyperparameters can be tested and evaluated.
As part of this, also different network architectures can be used.
Also, two other techniques have not been addressed so far in this approach:
  1. Prioritized Experience Replay
  2. Dueling Deep Q-Networks
