---
{"dg-publish":true,"permalink":"/supervised/expanded-explanations/lecture-8/"}
---


# Sequence Topologies and Parameter number

When structuring recurrent architectures for various processing tasks, understanding the underlying sequence topologies dictates how you shape your data tensors. Furthermore, grasping the exact parameter calculations is fundamental for optimizing the theoretical footprint of your models before training.

### 1. Sequence Topologies

Standard feed-forward networks (One-to-One) are restricted by fixed-size inputs and outputs, lacking the recurrency needed for sequential data. By introducing the loop mechanism, Recurrent Neural Networks (RNNs) can be unrolled across discrete time steps to handle dynamic sequences in several topological configurations:

- **One-to-Many:** The network receives a single, static input at the first time step and generates a sequence of outputs across subsequent time steps.
    
    - _Implementation:_ Image Captioning, where an image vector initializes the hidden state to sequentially predict words.
        
- **Many-to-One:** The network processes a sequence of inputs over multiple time steps, but only the final hidden state is used to calculate a single output.
    
    - _Implementation:_ Sequence Classification, such as determining the overall sentiment (positive/negative) of a complete text sequence.
        
- **Many-to-Many (Encoder-Decoder):** The network processes an input sequence of length $T_{in}$ into a single context vector (the encoder), which is then used to generate an output sequence of length $T_{out}$ (the decoder).
    
    - _Implementation:_ Language Translation, where the input and output sequences are of different lengths.
        
- **Many-to-Many (Synced):** An input sequence of length $T$ is processed to produce an output sequence of the exact same length $T$, with an output generated at every individual time step.
    
    - _Implementation:_ Named Entity Recognition, where every word in a sequence requires a corresponding classification tag.
        

---

### 2. Parameter Calculation

Unlike standard deep networks where weights only connect independent layers, RNNs introduce weight matrices that govern the temporal states. At any time step $t$, the output of an activation unit ($a_t$) depends on both the current input ($X_t$) and the previous activation output ($a_{t-1}$).

The hidden state update formula is defined as:

$$a_t = \tanh(X_t W_{ax} + a_{t-1} W_{aa} + b_1)$$

From this operation, we can mathematically derive the total number of trainable parameters for any hidden layer. Let $N$ be the number of units in the current hidden layer, and $I$ be the number of inputs coming into that layer.

**Hidden Layer Calculation:**

1. **Input Weights ($W_{ax}$):** Every input feature connects to every unit. Size = $I \times N$.
    
2. **Recurrent Weights ($W_{aa}$):** Every unit connects to every unit from the previous time step. Size = $N \times N$.
    
3. **Biases ($b_1$):** One bias per unit. Size = $N$.
    

Combining these, the parameters for a single RNN hidden layer is defined as:

$$P_{hidden} = N \times (I + N + 1)$$

**Output Layer Calculation:**

To generate the final prediction $\hat{Y}$, the final hidden state connects to the output nodes. If $O$ is the output size, the calculation requires $W_{ya}$ (weights) and $b_2$ (biases):

$$P_{output} = O \times (N + 1)$$

#### Example Derivation (Two Hidden Layers)

Following the mathematical walkthrough from the lecture involving a network with 1 Input Feature, a 3-unit Hidden Layer 1, a 4-unit Hidden Layer 2, and a 1-unit Output:

- **Hidden Layer 1:** 3 units $\times$ [1 (Input) + 3 (Recurrent) + 1 (Bias)] = **15 parameters**.
    
- **Hidden Layer 2:** 4 units $\times$ [3 (Inputs from HL1) + 4 (Recurrent) + 1 (Bias)] = **32 parameters**.
    
- **Output Layer:** 1 unit $\times$ [4 (Inputs from HL2) + 1 (Bias)] = **5 parameters**.
    
- **Total Network Parameters:** $15 + 32 + 5 =$ **52 parameters**.
    

Notice how the $N \times N$ recurrent connection causes the parameter count to scale quadratically with the size of the hidden dimension.

---

## "Every unit connects to every unit from the previous time step." Why so? Shouldn't it only be the same unit? Why all units in the layer 

It is very intuitive to assume that a single memory unit should only be responsible for its own continuous timeline.

However, if an RNN were built that way, it would severely limit the model's ability to understand complex data. To see why, we can look at the mathematics of the recurrent weight matrix ($W_{aa}$) and how networks build "distributed representations," particularly in sequence tasks like natural language processing.

### The Mathematical Difference

When calculating the hidden state at time $t$, the equation is:

$$a^{<t>} = \tanh(W_{ax}X^{<t>} + W_{aa}a^{<t-1>} + b)$$

- **If a unit only connected to itself:** The recurrent weight matrix $W_{aa}$ would be a **diagonal matrix** (all values are zero except the diagonal). The activation of unit 1 at time $t$ ($a_1^{<t>}$) would only be multiplied by $a_1^{<t-1>}$. It would have $N$ parameters instead of $N^2$.
    
- **Because it connects to all units:** $W_{aa}$ is a **dense matrix**. To calculate the new state for unit 1 ($a_1^{<t>}$), the network takes a weighted sum of $a_1^{<t-1>}$, $a_2^{<t-1>}$, $a_3^{<t-1>}$, all the way to $a_n^{<t-1>}$.
    

### Why is a Dense Matrix Necessary? (Feature Interaction)

In a neural network, each hidden unit learns to detect and track a specific, abstract "feature" within the data. The true power of deep learning comes from allowing these isolated features to interact with one another.

Imagine you are training an RNN language model to process the sentence: _"The cats, which the owner fed,..."_

The network needs to predict the next word (e.g., "are"). At any given time step, different units in the hidden layer are tracking different contextual clues:

- **Unit 1** might be tracking plurality (e.g., highly active if the main subject is plural).
    
- **Unit 2** might be tracking grammar syntax (e.g., highly active if we are currently inside a dependent clause like "which the owner fed").
    
- **Unit 3** might be tracking the semantic topic (e.g., highly active for "animals").
    

If the units only connected to themselves, they would act as completely isolated memory tracks. Unit 2 would know the dependent clause just ended, but it wouldn't know what the main subject was.

By fully connecting the layer, the network pools all of these individual features into a single, unified **Context Vector**. At the next time step, Unit 1 can update its own state based not just on its own past, but by looking at what Unit 2 and Unit 3 detected in the previous step. This cross-communication allows the network to logically deduce: _"The dependent clause just ended (Unit 2's past state), and the main subject was a plural animal (Unit 1 and Unit 3's past states), therefore the next expected feature should be a plural verb."_

In short, connecting every unit to every unit allows the RNN to maintain a holistic, interacting "state of the world" rather than a collection of blind, parallel tracks.

---

# RNN Vs GRU Vs LSTM

To understand how Recurrent Neural Networks (RNNs) evolve into more advanced models, it is helpful to look at how they manage the flow of information over time. Standard RNNs struggle with maintaining long-term context, which led to the introduction of specialized "gating" mechanisms to explicitly control what information is kept, updated, or discarded.

### 1. The Gated Recurrent Unit (GRU)

The GRU introduces a modification to the standard RNN by adding an **Update Gate** ($G_u$). This gate acts as a filter that determines how much of the past knowledge needs to be passed along to the future.

- **Calculating the Gate:** The value of the Update Gate ($G_u$) is calculated based on two primary inputs: the current input data at time $t$ ($X^{<t>}$) and the previous memory cell value ($C^{<t-1>}$).
    
- **Trainable Parameters:** To learn how to effectively use this gate, the network trains specific weight matrices, notably $W_{ux}$ (for the input) and $W_{uc}$ (for the memory), alongside a bias ($b_u$) and a Sigmoid activation function to keep the gate's value between 0 and 1.
    
- **Memory Update:** Simultaneously, the network calculates a "Candidate Value" for the new memory ($\tilde{C}^{<t>}$) using a $\tanh$ function. The final, updated memory for the current time step ($C^{<t>}$) is a blend of the old memory and the new candidate, strictly controlled by the Update Gate:
    
    $$C^{<t>} = G_u \cdot \tilde{C}^{<t>} + (1 - G_u) \cdot C^{<t-1>}$$
    

### 2. Long Short-Term Memory (LSTM)

The LSTM architecture builds upon the gating concept of the GRU but introduces more granular control over the memory state through two major structural changes.

**A. Splitting the Gates** Instead of using a single Update Gate to govern both what is added and what is removed, the LSTM splits this responsibility into two independent gates:

- **The Update Gate ($G_u$):** Determines how much of the _new_ candidate memory should be stored. It uses its own set of weights ($W_{ux}$, $W_{uc}$) and bias ($b_u$).
    
- **The Forget Gate ($G_f$):** Determines how much of the _old_ memory ($C^{<t-1>}$) should be discarded or retained. It operates with a separate set of weights ($W_{fx}$, $W_{fc}$) and bias ($b_f$).
    

**B. Differentiating Memory from Output** In a GRU, the internal memory cell value is often exposed directly to the subsequent classification layers (like a Softmax layer). However, the lecture notes a critical warning regarding LSTMs: simply combining the independent Update and Forget gates ($G_u \cdot \tilde{C}^{<t>} + G_f \cdot C^{<t-1>}$) results in a value that is **not bounded by $[-1, 1]$**.

To solve this, the LSTM explicitly differentiates between the internal **Cell Memory ($C^{<t>}$)** and the visible **Cell Output ($a^{<t>}$)**.

- Before the internal memory is passed to the next layer or the Softmax classifier, it is "squashed" using a hyperbolic tangent function.
    
- This results in the final output equation: $a^{<t>} = \tanh(C^{<t>})$, ensuring the final visible output strictly remains within the $[-1, 1]$ range.