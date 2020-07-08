# Optimization

### Papers: 
1. [Escaping From Saddle Points Online Stochastic Gradient for Tensor Decomposition](http://proceedings.mlr.press/v40/Ge15.pdf)
2. [How to Escape Saddle Points Efficiently](https://arxiv.org/pdf/1703.00887.pdf)
3. [No spurious local minima in  nonconvex low rank problems: A unified geometric analysis](https://arxiv.org/pdf/1704.00708.pdf)

### Important points:
1. Optimizing a non-convex function is NP-hard in general. The difficulty comes from two aspects.   

- First,the function may have many local minima, and it might be hard to find the best one (global minimum) among them.
- Second, even finding a local minimum might be hard as there can be many saddle points which have 0-gradient but are not local minima. In many cases, especially in those related to deep neural networks, 
the main bottleneck in optimization is not due to local minima, but the existence of many saddle points.

2. Adding noise to gradient (pertubed SGD) could improve the optimization of the deep neural networks.    
-> Randomness in SGD helps the algorithm to escape from the saddle points in polynomial time.

3. 2nd order optimization for deep learning?    
- The idea is to inject the 2nd order optimization in order to find the local minima more quickly 
-> We can compute an approximate backpropagation of second order term in linear time, but empirically the 2nd order doesn't seem to find a better quality nets so far.

4. Convergence to global optimum from arbitrary initial points     
In many problems of interest, all local minima are global minima  
- matrix completion/recommendation/collaborative filtering
- tensor decomposition 
- dictionary learning 
- phase retrieval
- matrix sensing 

# Generalization
### Papers: 
1. [Understanding deep learning requires rethinking generalization ](https://arxiv.org/pdf/1611.03530.pdf)
2. [To Understand Deep Learning We Need to UnderstandKernel Learning](https://arxiv.org/pdf/1802.01396.pdf)
3. [Stronger generalization bounds for deep nets via a compressionapproach](https://arxiv.org/pdf/1802.05296.pdf)
4. [Proving generalization of deep nets via compression](http://www.offconvex.org/2018/02/17/generalization2/)

### Important points:
1. Overparametrization may help optimization
An interesting experiment as folows:
- First we generate labeled data by feeding random inputs vectors into a neural networks depth 2 layers with hidden layer of size n
- The we try to fit a new neural networks 2 layers with the same number of hidden nodes to this labelled dataset 
-> We found that it is difficult to train this way, but much easier to train this labeleed dataset with a new net with bigger hidden layer (even though still no theorem explaining this)

2. A series of very interesting experiments conducted in the paper [Understanding deep learning requires rethinking generalization](https://arxiv.org/pdf/1611.03530.pdf) show that:

*   Deep neural networks easily fit random labels. Optimization on random labels remains easy. In fact, training time increases only by a small constant factor compared with training on the true labels.

*   By varying the amount of randomization, interpolating smoothly betweenthe case of no noise and complete noise in dataset, they observe a steady deterioration of the generalization error as we increase the noise level.   
This shows that neural networks are able to capture the remaining signal in the data, while at the same time fit the noisy part using brute-force.  
This conclusion is confirmed by an experiment did by CIB where they use 98% noisy data et 2% true data, the Bert model is still able to fit and obtains a good result.

*   Explicit regularization such as weight decay, dropout, and data augmentation may improve generalization performance, but is neither necessary nor by itself sufficient for controlling generalization error.

*   SGD acts as an implicit regularizer.

3. Effective capacity of a model?   
If training set had m samples then the generalization error (error on training data - error on test data) is of the order of racine(N/m).   
Here N is the number of effective parameters (or complexity measure) of the net; it is at most the actual number of trainable parameters but could be much less.

This theorem can lead to:

*    The compression approach takes a deep net C with N trainable parameters and tries to compress it to another one C' that has (a) much fewer parameters N' than C and (b) has roughly the same training error as C.
So long as the number of training samples exceeds N', then C' does generalize well (even if C doesn’t)

*    A flat minimum should generalize better than a sharp minimum on deep neural networks. Suppose a flat minimum is one that occupies “volume” τ in the landscape. (The flatter the minimum, the higher τ is.) Then the number of distinct flat minima in the landscape is at most S=total volume/τ. 
Thus one can number the flat minima from 1 to S, implying that a flat minimum can be represented using logS bits. The above-mentioned basic theorem implies that flat minima generalize if the number of training samples m exceeds logS.

4. Noise stability for deep nets  
If we inject appropriately scaled gaussian noise at the output of some layer, then this noise gets attenuated as it propagates up to higher layers.  This is related to notions like dropout, though it arises also in nets that are not trained with dropout.


# Role of depth

### Papers


# Unsupervised learning, GANs
### Papers 
1. [Representation Learning: A Review and New Perspectives](https://arxiv.org/pdf/1206.5538.pdf)

### Important points
1. Manifold assumption: data presented in high dimensional spaces are expected to concentrate in the vicinity of a manifold $`M`$ of much lower dimensionality $`d_M`$, embedded in high dimensional input space
$`R^{d_x}`$. Learning the structure of the manifold becomes an important task, addtionally this learning task seems to be possible without the use of labeled training data.   
With this perspective, the primary unsupervised learning task is then seen as modeling the structure of the data-supporting manifold. The associated representation being learned can be associated with
an intrinsic coordinate system on the embedded manifold. One of the most widely used approaches is PCA (principal component analysis) which models a linear manifold.

2. The goal of unsupervised learning motivation is the high level representation (manifold space) is good substitue for input X in downstream tasks. Then solving those tasks requires fewer labeled samples if use Z instead of X.

3. Deep generative models  
- An AutoEncoder starts by explicitly defining a feature-extracting function in a specific parametrized closed form (called Encoder): $`h = f_{\theta}(x)`$ where $`h`$ is e feature-vector or representation or code computed from $`x`$.
Another closed form parametrized function $`g_{\theta}`$, called the decoder, maps from feature space back into input space, producing a reconstruction $`r = g_{\theta}(h)`$.  The set of parameters $`\theta`$ of the encoder and decoder are learned simultaneously on the task
of reconstructing as well as possible the original input, i.e. attempting to incur the lowest possible reconstruction error $`L(x, r)`$ – a measure of the discrepancy between $`x`$ and its reconstruction $`r`$ – over training examples.

Notes: To capture the structure of the data-generating distribution, it is therefore important that something in the training criterion or the parametrization prevents the auto-encoder from learning the identity function, which has zero reconstruction error everywhere
This is achieved through various means in the different forms of auto-encoders, as described below in more detail, and we call these regularized auto-encoders:
- Sparse Auto-Encoders
- Denoising Auto-Encoders: denoising an artificially corrupted input, i.e. learning to reconstruct the clean input from a corrupted version. Learning the identity is no longer enough: the learner must capture the structure of the input distribution in order to optimally undo the effect of the corruption process, with the reconstruction essentially
being a nearby but higher density point than the corrupted input. 
- Contractive Auto-Encoders: adding an analytic contractive penalty to the Frobenius norm of the encoder’s Jacobian, and results in penalizing the sensitivity of learned features to
infinitesimal input variations. 
- Variational Autoencoder: 

4.  Generative Adversarial Nets (GANs)


