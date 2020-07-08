## Papers:
1. [Representation Learning on Graphs: Methods and Applications](https://www-cs.stanford.edu/people/jure/pubs/graphrepresentation-ieee17.pdf)
2. [Anonymous Walk Embeddings](https://arxiv.org/pdf/1805.11921.pdf)

## Important points
1. The idea behind these representation learning approaches is to learn a mapping that embeds nodes, or entire (sub)graphs, as points in a low-dimensional vector space. The goal is to optimize this mapping so that geometric relationships in this learned space reflect the structure of the original graph. 
After optimizing the embedding space, the learned embeddings can be used as feature inputs for downstream machine learning tasks.

1. **Random walks**: are the sequences of nodes, where each new node is selected independently from the set of neighbors of the last node in the sequence. A random walk is a finite Markov chain. 
In principle, we will estimate the probability of visisting node `v` on a random walk starting from node `u` using some random walk strategy `R`.
The key innovation is optimizing the node embedding so that nodes have similar embeddings if they tend to co-occur on short random walks over the graph. It shows the efficiency because it do not need to consider all the nodes pairs when training,
**only need to consider pair that co-occur on random walks**. Moreover, it **incoporates the information from both local and high-order neighborhood information**.

Methods:
- [DeepWalk](https://arxiv.org/pdf/1403.6652.pdf)
- [Node2vec](https://arxiv.org/pdf/1607.00653.pdf)

2. **Anonymous walks**:   
Normally states in a random walk correspond to a label or a global name of a node; however such states could be unavailable. Global  topology  of  the  network may  be  hidden  deliberately  (e.g. social  networks  often restrict outsiders to examine your friendships) 
or otherwise(e.g. newly created links in the world wide web may be yet unknown to the search engine).   
Anonymized version of a random walk can provide a flexible way to reconstruct a network even when global names are absents.   

3. **Negative samplling**    
Instead of normalizing over the full vertexset which costs time and ressources, this method approximates the normalizing factor using a set of k random “negative samples”


