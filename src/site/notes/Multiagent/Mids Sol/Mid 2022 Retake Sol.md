---
{"dg-publish":true,"permalink":"/multiagent/mids-sol/mid-2022-retake-sol/"}
---


For vacuum cleaner world: your agent is a vacuum cleaner robot, which is responsible for cleaning 4 rooms (A, B, C, and D). Each state is represented by the agent’s location and the dirt status at this location. The goal state is achieved when all rooms are clean. You are required to do the following tasks:

1. Create the state space for all possible states. 
	1. Comment on the state space type if it is cyclic or acyclic.

2. Consider the initial state is the corresponding figure a state (C’). Build a search tree with at least depth of 3 levels. 
	1. Comment on expected depth of complete tree.

![Pasted image 20260406184304.png](/img/user/imgs/Pasted%20image%2020260406184304.png)

# Sol: 

### **1. State Space**

Based strictly on the rule ("Each state is represented by the agent’s location and the dirt status at this location"), the state space is defined by 8 distinct simplified states.

Let a letter represent a clean room and a prime (`'`) represent a dirty room:

- **State Space:** `{A, A', B, B', C, C', D, D'}`
    

_(Note: While these 8 codes represent the agent's immediate local state, the environment must implicitly track the dirt in all four rooms behind the scenes to know when the "all rooms are clean" goal is achieved. A fully observable global state space would contain $4 \times 2^4 = 64$ states, every combination of "Cleanness", for each of the 4 rooms)._

**State Space Type:**

The state space is **Cyclic**. The agent can move back and forth between rooms indefinitely without performing a "Suck" action (e.g., moving $C' \rightarrow A' \rightarrow C' \rightarrow A'$), creating infinite loops in a search tree if visited states are not tracked.

---

### **2. Search Tree (Depth of 3 Levels)**

**Environment Layout:**

- **Top Row:** A (Dirty), C (Dirty)
    
- **Bottom Row:** B (Clean), D (Dirty)
    
- _Possible Moves:_ Up, Down, Left, Right (bounded by walls).
    

**Initial State:** `C'` (Agent in C, C is dirty. Uncleaned rooms: A, C, D)

Here is the search tree expanded to 3 levels. To make the tree accurate to the goal, I have tracked the hidden global dirt states to determine what code the agent encounters when moving.

- **Level 0**
    
    - **C'** * **Level 1** _(Actions from C')_
        
    - ├── **Suck** $\rightarrow$ **C** _(Rooms A, D remain dirty)_
        
    - ├── **Left** $\rightarrow$ **A'** _(Rooms A, C, D remain dirty)_
        
    - └── **Down** $\rightarrow$ **D'** _(Rooms A, C, D remain dirty)_
        
- **Level 2** _(Actions from Level 1)_
    
    - _From C:_
        
        - ├── **Left** $\rightarrow$ **A'** _(Rooms A, D remain dirty)_
            
        - └── **Down** $\rightarrow$ **D'** _(Rooms A, D remain dirty)_
            
    - _From A':_
        
        - ├── **Suck** $\rightarrow$ **A** _(Rooms C, D remain dirty)_
            
        - ├── **Right** $\rightarrow$ **C'** _(Cycle)_
            
        - └── **Down** $\rightarrow$ **B** _(Rooms A, C, D remain dirty)_
            
    - _From D':_
        
        - ├── **Suck** $\rightarrow$ **D** _(Rooms A, C remain dirty)_
            
        - ├── **Up** $\rightarrow$ **C'** _(Cycle)_
            
        - └── **Left** $\rightarrow$ **B** _(Rooms A, C, D remain dirty)_
            
- **Level 3** _(Actions from a selection of Level 2 nodes)_
    
    - _From C $\rightarrow$ A':_
        
        - ├── **Suck** $\rightarrow$ **A** _(Only D remains dirty)_
            
        - ├── **Right** $\rightarrow$ **C** _(Cycle)_
            
        - └── **Down** $\rightarrow$ **B** _(Rooms A, D remain dirty)_
            
    - _From C $\rightarrow$ D':_
        
        - ├── **Suck** $\rightarrow$ **D** _(Only A remains dirty)_
            
        - ├── **Up** $\rightarrow$ **C** _(Cycle)_
            
        - └── **Left** $\rightarrow$ **B** _(Rooms A, D remain dirty)_
            
    - _From A' $\rightarrow$ A:_
        
        - ├── **Right** $\rightarrow$ **C'** _(Rooms C, D remain dirty)_
            
        - └── **Down** $\rightarrow$ **B** _(Rooms C, D remain dirty)_
            

![Pasted image 20260406190737.png](/img/user/imgs/Pasted%20image%2020260406190737.png)

---

### **Comment on the Expected Depth of the Complete Tree**

- **Optimal Path Depth:** The expected depth to reach the goal state (all rooms clean) along the most efficient path is **6 levels**.
    
    - _Optimal Sequence:_ Suck at C (1) $\rightarrow$ Move Left to A (2) $\rightarrow$ Suck at A (3) $\rightarrow$ Move Down to B (4) $\rightarrow$ Move Right to D (5) $\rightarrow$ Suck at D (6).
        
- **Complete Tree Depth:** Because the state space is cyclic, the complete unpruned search tree has an **infinite depth**. The agent can wander forever without cleaning. If a search algorithm uses cycle-checking (tracking visited global states), the absolute maximum depth of the tree would be capped at 64.