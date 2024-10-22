PHYSICS INFORMED NEURAL NETWORK

**Problem Statement:**

The study aims to address the computational challenges associated with
simulating particle interactions in particle systems by proposing a
novel approach inspired by physics and graph theory. Specifically, it
seeks to model particle interactions as a graph network, where particles
represent individuals and interactions are represented by graph edges.
The objective is to leverage Graph Neural Network (GNN) processors to
learn and predict particle trajectories efficiently. By doing so, we can
help resolve the problem of object morphing and other physics issues in
video generation models such as SORA.

**Introduction:**

Particle-based simulations are vital tools for understanding complex
physical phenomena, ranging from fluid dynamics to structural mechanics.

However, these simulations often pose computational challenges due to
the intricate interactions between particles and the computational
resources required to model them accurately. To address these
challenges, this study introduces a novel approach to modelling particle
interactions in fluid systems through physics and graph theory. By
conceptualizing particles as individuals within a social network and
their interactions as edges in a graph, the study proposes to leverage
Graph Neural Networks (GNNs) to learn and predict particle trajectories
efficiently. This approach not only offers the potential for improved
computational efficiency but also provides insights into the underlying
dynamics of particle interactions. By integrating Graph Neural Networks
(GNNs) into particle-based simulations, our objective is to enhance the
computational efficiency, precision, and interpretability of modelling
complex fluid dynamics.

**Algorithm:**

Model Architecture

1.  Building node_features and edge_features:

-   For node_features we use velocity at each timestamp, the distance
    > from border and embedding

-   For edge_features we use distance between particles and a relative
    > displacement.

2.  Encoder:

-   Has 2 separate networks for processing node_features and
    > edge_features. Converts both to a dimension of 128.

3.  Processor:

-   The processor iterates through multiple message passing steps to
    > capture interactions among particles.

-   At each step, the processor updates the particle states and edge
    > features based on the interactions observed in the previous step.
    > This iterative process allows information to propagate through the
    > graph, enabling particles to influence each other\'s states.

4.  Decoder:

-   Has a network for processing the node_features, because at last we
    > only need the acceleration of each particle/node of the graph.

5.  Output:

-   Final output is compared with an estimated acceleration using the
    > last time stamp position of the particles. Loss function is MSE.

MESSAGE PASSING

> The Message Passing network uses the concept of multidimensional edge
> feature(MP-GNN). Which uses the node_features of the target(x_j) and
> source(x_i) and edge_feature matrix. Using the aforementioned
> information three features vectors are calculated as follows:

-   Target to Source information flow

-   Source to Target information flow

-   Edge features

> After receiving these values they are concatenated and passed through
> the edge function(an MLP model) to compute new edge features.
> Internally an update function is called that updates the node_features
> during message passing.

**DataSet:**

({\'particle_type\': \<tf.Tensor: shape=(283,), dtype=int64, numpy=

array(\[7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7,
7,

7, 7, 7, 7, 7, 7, 7, 7, 7,... 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7,

7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7\])\>, \'key\':
\<tf.Tensor: shape=(), dtype=int64, numpy=0\>}, {\'position\':
\<tf.Tensor: shape=(401, 283, 2), dtype=float32, numpy=

array(\[\[\[0.31905454, 0.3713607 \],

\[0.3085667 , 0.37355483\],

\...,

\[0.1936751 , 0.10736296\]\]\], dtype=float32)\>})

**RESULTS & DISCUSSION**

**Best Performing Model**

-   **Epochs : 106500**

-   **NumParticles : 1**

-   **Loss : Based on acceleration**

-   **InputTimestamps : 101**

-   **Outputs for train**
  
 > ![](media/image4.gif)
 > ![](media/image9.gif)

-   **Outputs for test data**

![](media/image6.gif)
![](media/image1.gif)


**Other Models:**

**Model 1:**

-   **Epochs : 128000**

-   **NumParticles : 9(with embedding)**

-   **Loss : Based on acceleration**

-   **TimeStamp : 6**

-   **Outputs on train**
  
> ![](media/image7.gif)

**Model 2:**

-   **Epochs : 62500**

-   **NumParticles : 9(with embedding)**

-   **Loss : Based on acceleration**

-   **TimeStamp : 6**

-   **Outputs on test data**

> ![](media/image5.gif)

**Model 3:**

-   **Epochs : 2000**

-   **NumParticles : 1**

-   **Loss : Based on position**

-   **TimeStamp : 6**

-   **Rollout : Predict positions**

-   **Outputs on train data**

> ![](media/image3.gif)

**Model 4:**

-   **Epochs : 10000**

-   **NumParticles : 1**

-   **Loss : Based on position**

-   **Rollout : Predict acceleration**

-   **TimeStamp : 6**

-   **Outputs on train data**

> ![](media/image2.gif)

**Model 5:**

-   **Epochs : 6000**

-   **NumParticles : 1**

-   **Loss : Based on position**

-   **TimeStamp : 20**

-   **Outputs on train data**
  
  >  ![](media/image8.gif)

**LIMITATION**

For the best performance of the model it is to be trained for a large
number of epochs. The authors had trained the model on 20 million
epochs.

**CONCLUSION**

It is possible to train the model to learn actual physics. But, the
results produced were not up to the mark. Further, a system with
different types of particles can also be trained, for the model to
generalize the behavior of different types of particles. While keeping
in mind some common attributes like free falling under gravity, but
specializing for particle characteristics such as viscosity/friction.

**REFERENCES**

1.  [[https://arxiv.org/abs/2002.09405]{.underline}](https://arxiv.org/abs/2002.09405)

2.  [[https://github.com/google-deepmind/deepmind-research/blob/master/learning_to_simulate/]{.underline}](https://github.com/google-deepmind/deepmind-research/blob/master/learning_to_simulate/)
