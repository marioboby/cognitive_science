---
{"dg-publish":true,"permalink":"/reasoning/finals-sol/final-25-sol/"}
---


# Q2
## **A) Use the Resolution Algorithm to prove that "Alice is accepted"**

**1. Define Predicates and Constants:**

- $H(x)$: $x$ has high grades
    
- $R(x)$: $x$ has strong recommendations
    
- $A(x)$: $x$ is accepted
    
- $W(x)$: $x$ works hard
    
- $a$: Alice
    

**2. Translate the story into First-Order Logic:**

1. Everyone who has high grades or has strong recommendations is accepted:
    
    $$\forall x \, ((H(x) \lor R(x)) \rightarrow A(x))$$
    
2. Everyone who works hard has high grades:
    
    $$\forall x \, (W(x) \rightarrow H(x))$$
    
3. Alice did not work hard but she has strong recommendations:
    
    $$\neg W(a) \land R(a)$$
    

- **Goal to prove:** $A(a)$
    

**3. Convert to Conjunctive Normal Form (CNF):**

- **From Premise 1:**
    
    - $\neg (H(x) \lor R(x)) \lor A(x)$
    
    - Apply De Morgan's Law: $(\neg H(x) \land \neg R(x)) \lor A(x)$
    
    - Distribute: $(\neg H(x) \lor A(x)) \land (\neg R(x) \lor A(x))$
    
    - **C1:** $\neg H(x) \lor A(x)$
        
    - **C2:** $\neg R(y) \lor A(y)$
        
- **From Premise 2:**
    
    - **C3:** $\neg W(z) \lor H(z)$
        
- **From Premise 3:**
    
    - **C4:** $\neg W(a)$
        
    - **C5:** $R(a)$
        
- **Negated Goal:**
    
    - **C6:** $\neg A(a)$
        

**4. Resolution Steps:**

We only need a few steps to reach a contradiction because her strong recommendations alone are sufficient for acceptance.

- **Step 1:** Resolve **C6** ($\neg A(a)$) with **C2** ($\neg R(y) \lor A(y)$) by substituting $y = a$:
    
    - _Result:_ **C7:** $\neg R(a)$
        
- **Step 2:** Resolve **C7** ($\neg R(a)$) with **C5** ($R(a)$):
    
    - _Result:_ **$\square$ (Contradiction)**
        

**Conclusion:** By deriving the empty clause, we prove the original goal is true. **Alice is accepted.**

---

### **B) Translate the following statements into predicate logic**

**Constants:** $s$ (Sarah), $t$ (Tom), $m$ (Mike)

**Predicates:** $Animal(x)$, $Loves(x,y)$, $Dog(x)$, $Cat(x)$, $Barks(x)$, $Dangerous(x)$, $Owns(x,y)$, $Friendly(x)$

**a. Sarah loves every animal.**

$$\forall x \, (Animal(x) \rightarrow Loves(s, x))$$

**b. Dogs and cats are animals.**

$$\forall x \, ((Dog(x) \lor Cat(x)) \rightarrow Animal(x))$$

**c. Everything that barks and is not dangerous is a dog.**

$$\forall x \, ((Barks(x) \land \neg Dangerous(x)) \rightarrow Dog(x))$$

**d. Tom owns a dog and it's friendly.**

$$\exists x \, (Dog(x) \land Owns(t, x) \land Friendly(x))$$

**e. Mike owns every pet that Tom owns.**

$$\forall x \, (Owns(t, x) \rightarrow Owns(m, x))$$

_(Note: Assuming we don't need a Predicate for Pet)._

---

### **C) Prove or disprove the goal `friend1(X) & friend2(X)` by using backward chaining (Look Lecture 4).**

**Initial Facts Database:**

- $h1 = True$
    
- $h2 = True$
    

**Goal:** Prove `friend1(X) , friend2(X)`

**Evaluating Sub-goal 1: Prove `friend1(X)`**

- **Rule 1** says IF $h$ is true THEN $Friend1(X)$ is true.
    
    - _Check Database:_ $h$ is not in the database. Rule 1 fails.
        
- **Rule 2** says IF $a$ is true AND $b$ is true THEN $Friend1(X)$ is true.
    
    - _Attempt to prove `a`:_
        
        - **Rule 4** says IF $not(d)$ is true THEN $a$ is true. (This requires $d$ to be false).
            
        - _Attempt to prove `d`:_
            
            - **Rule 8** says IF $h1$ is true AND $h2$ is true THEN $d$ is true.
                
            - _Check Database:_ $h1$ is true, and $h2$ is true. Therefore, **$d$ is true**.
                
        - Since $d$ is true, $not(d)$ is false. Therefore, **Rule 4 fails**, meaning **$a$ is false**.
            
    - Because $a$ is false, the condition for Rule 2 ($a \land b$) fails. Rule 2 fails.
        
- **Conclusion for Sub-goal 1:** $Friend1(X)$ cannot be proven and is evaluated as **False**.
    
---

**Evaluating Sub-goal 2: Prove `friend2(X)`**

- Matching friend2(X):

- R3: IF c(X) is true THEN Friend2(X) is true

	- Matching c(X) :

	- R7: IF not(e) is true THEN c(2) is true

		- Matching e 

		- R9: IF h2 is true  AND h3 is true THEN e is true

		- h2 is true and h3 is false then e is false then R9 is false

	- Return to R7:

	- R7: IF not(e) is true THEN c(2) is true

	- e is false, then not(e) become true
	
	- So, c(2) is true then R7 is true 

- Return to R3:

- R3: IF c(X) is true THEN Friend2(X) is true

 - c(X) is true for X=2 

then Friend2(X) is true where X=2
