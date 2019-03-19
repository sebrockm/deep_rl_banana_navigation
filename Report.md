# Report

## Algorithm
This project uses a Deep Q-Network algorithm.

That means a neural network is used to approximate the Q-table:

### The neural network
The network used in this solution has three layers:
  1. A recurrent layer (GRU) with 100 hidden state size of 100
  2. A fully connected layer with 150 units
  3. A output layer with 4 units, which is the size of the action space
  
Between the layers 1 and 2 and between the layers 2 and 3 ReLU activation and dropout with a rate of 0.2 is applied.

The algorithm uses two identical networks of these, to apply the **Fixed Q-Targets** technique.
This technique uses a the second (fixed) network to calculate the target values that the loss function needs to calculate an error (medium quared error is used in this project).
This error is used to calculate the gradients needed to update the weights of the first network. The learning rate was chosen to be **lr=0.001**.
