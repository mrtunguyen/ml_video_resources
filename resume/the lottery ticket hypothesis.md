## Papers
1. [The lottery ticket hypothesis: finding sparse, trainable neural networks](https://arxiv.org/pdf/1803.03635.pdf)
2. [The Lottery Ticket Hypothesis at Scale](https://arxiv.org/pdf/1903.01611v1.pdf)
3. [Understanding the generalization of lottery tickets in neural networks](https://ai.facebook.com/blog/understanding-the-generalization-of-lottery-tickets-in-neural-networks/), [paper](https://arxiv.org/pdf/1906.02773.pdf)
4. [Deconstructing Lottery Tickets: Zeros, Signs, and the Supermask](https://eng.uber.com/deconstructing-lottery-tickets/), [paper](https://arxiv.org/pdf/1905.01067.pdf)
5. [Stabilizing the Lottery Ticket Hypothesis](https://arxiv.org/pdf/1903.01611v2.pdf)

## Important points
1. **Lottery ticket hypothesis**: proposes that randomly-initialized, dense neural networks contain much smaller, fortuitously initialized  subnetworks  (**winning tickets**)  capable  of training to similar accuracy as the original network at a similar speed. This method can reduce the parameter counts of trained networks by over 90%, decreasing storage requirements and improving computational performance of training and inference without compromising accuracy.

2. Algorithm for winning a lottery ticket:   
- Randomly initialize a full neural network $`f(x;{\theta}_0)`$
- Train the network for $`j`$ iterations, arriving at parameters $`{\theta}_j`$
- Prune $`p\%`$ of the parameters in $`{\theta}_j`$ whose magnitude weights are smaller than a threshold, creating a mask $`m \in \{0,1\}^{|\theta|}`$.
- Reset the remaining parameters to their initialized values in $`{\theta}_0`$, creating the winning ticket $`f(x; m \bigodot {\theta}_0)`$

In a nutshell, the idea is as follow: after training a network, set all weights smaller than some threshold to zero (prune them), rewind the rest of the weights to their initial configuration, and then retrain the network from this starting configuration keeping the pruned weights weights frozen (not trained)

3. **The network trains well only if it is rewound to its initial state, including the specific initial weights that were used. Reinitializing it with new weights causes it to train poorly**. The specific combination of pruning mask (a per-weight binary value indicating whether or not to delete the weight) and weights underlying the mask form a lucky sub-network found within the larger network, or, as named by the original study, a winning “Lottery Ticket.”

4. There are still some challenges to find winning tickets for deeper networks. A solution for this is that after training and pruning the network, do not reset each weight to its initialization at the beginning of training; instead, reset it to its value at an iteration very close to the beginning of training. this practice is called **late resetting**,since the weights that survive pruning are reset back to their values at an iteration slightly later than initialization.

5. Another interesting finding is that many of the rewound, masked networks without training had accuracy significantly better than chance at initialization. 

6. A recommendation for choosing a weight mask is freezing all weights to zero if they moved toward zero over the course of training. 

7. A question for generalization problem in transfer learning with the lottery ticket models: Should we can get a similar results with transfer leanring in the lottery ticket as with the full NN? Some experiments have shown that they still get a good results while doing transfer learning with the lottery ticket hypothesis, for example in BERT models.