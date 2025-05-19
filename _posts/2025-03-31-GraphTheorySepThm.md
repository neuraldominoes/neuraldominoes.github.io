---
title: "Tree Separator Theorem"
date: 2025-03-31
math: true  
tags: [graph-theory]
---

# Tree Separator Theorem

Here is a random question. Actually, let's state multiple different questions involving the same concept:

**A)** If we delete a node $v$ from a tree (together with all edges that end there), we get a graph whose connected components are trees. We call these connected components the branches at node $v$. Prove that every tree has a node such that every branch at this node contains at most half the nodes of the tree.

**B)** Any $n$-vertex binary tree can be separated into two subtrees, each with no more than $\frac{2n}{3}$ vertices, by removing a single edge.

**C)** Any $n$-vertex tree can be divided into two parts (subgraphs that can be forests), each with no more than $\frac{2n}{3}$ vertices, by removing a single vertex.

## Separator Theorems for Trees

- $S$: subset of vertices in $G$
- $H$: subgraph  
We say $S$ *separates* $H$ in $G$ if there is no edge between any of $V(H)$ and $V(G) - V(H) - S$.

---

1. **Given:**
   - A real number $\alpha > \frac{1}{2}$.
   - A tree $T$ with at least $\alpha + 1$ vertices.  
   - **Then:**
     There exists a vertex $v$ in $T$ whose removal will disconnect $T$ into a forest (a collection of smaller trees), where one of the resulting components $F$ satisfies: $\alpha \leq \|V(F)\| < 2\alpha$

   **Note:** $\alpha$ is just a parameter used to set the size bound. No matter what $\alpha > \frac{1}{2}$ you choose, as long as the tree has at least $\alpha + 1$ vertices, you can find such a separator vertex $v$.

   We choose $\alpha = \frac{1}{2}$ so that $2\alpha = 2 \times \frac{1}{2} = 1$.  
   If $\alpha \leq \frac{1}{2}$, the theorem could become trivial or impossible to satisfy. For example:  
    - If $\alpha = 0.4$, then $2\alpha = 0.8$, and the only integer satisfying $0.4 \leq \|V(F)\| < 0.8$ is $\|V(F)\| = 0$, which is not useful (a component with $0$ vertices doesn't make sense).  
    - With $\alpha > \frac{1}{2}$, the interval $[\alpha, 2\alpha)$ will always contain at least one integer (specifically, $1$), making the theorem non-trivial.

## Proof

We basically construct a separator vertex by iteratively refining our choice of the separator vertex till we get what we want. 

1. **Base Case:**  
  - If $\alpha \leq 1$, we are done. If we just remove a leaf from the tree, we get a single vertex as one component, which satisfies the inequality:

$$
\alpha \leq |V(F)| < 2\alpha
$$
   
   - let's also knock the reason for having $\|V(T)\| \geq \alpha + 1$ as a constraint out of the way. 
   - In a tree with fewer than $\alpha + 1$ vertices, removing any vertex leaves components of size at most $\alpha$, so the theorem holds trivially.

Now onto the more juicy stuff since we've got that out of the way. 

2. **Inductive Step:**  
   - Pick a leaf $v_0$  this has a neighbour $v_1$. 
   - Remove $v_1$ hehehhe. This splits the tree into several components:
     - One component is just $v_0$ (size $1$).  
     - The other components are subtrees not containing $v_0$, denoted $T_1, T_2, \dots$.  

   > *(We now get a bunch of components as the graph falls apart into a bunch of components in our hands. (im being dramatic af here, but think of it as just removing a node in a mutistemmed flower and the different pieces falling apart in your hand.))*  
   > note to self insert that one pic of magnolias or wtv btw 

   - **Case 1:** If all $T_i$ happen to be such that $\|V(T_i)\| \leq \alpha$, then we can take a union of some of them to form $F$ such that:
     
$$
\alpha \leq |V(F)| < 2\alpha
$$
 
   This is definitely also possible because $\|V(T)\| > \alpha + 1$.  

   - **Case 2:** If some $T_1$ has $\|V(T_1)\| > \alpha$:  
     - If $\|V(T_1)\| < 2\alpha$, then $F = T_1$ satisfies the theorem, and we are done.  
     - If $\|V(T_1)\| \geq 2\alpha$, we **recurse** on $T_1$ ​ in a similar manner to what we've done thus far:  
       - Let $v_2$ be the neighbor of $v_1$ inside $T_1$.  
       - Remove $v_2$ and consider the components $S_1$ not containing $v_1$.  
       - The total vertices in $S_1$ must be at least $2\alpha - 1$ (since $T_1$ originally had $\geq 2\alpha$ vertices).  
       - Repeat the process:  
         - If all subtrees in $S_1$ have $\leq \alpha$ vertices, combine them to form $F$.  
         - If some subtree $T_2$ in $S_1$ has $\geq \alpha$ but $< 2\alpha$ vertices, we are done.  
         - If some $T_2$ has $\geq 2\alpha$ vertices, let $v_3$ be its neighbor and continue.  

   By repeating this, the tree gets smaller at each step, ensuring we eventually hit a base case or find the desired separator vertex.  

## Application to one of the original questions

Consider the problem:  
"Any $n$-vertex tree can be divided into two parts (subgraphs that can be forests), each with no more than $\frac{2n}{3}$ vertices, by removing a single vertex."

We can solve this using the theorem:  
- Set $\alpha = \frac{n}{3}$.  
- Since $\alpha > \frac{1}{2}$ for $n \geq 2$, the theorem applies.  
- The tree $T$ has $\|V(T)\| = n \geq \alpha + 1 = \frac{n}{3} + 1$, which holds for $n \geq 2$.  

Alternatively, we can run through the method directly:  
1. Start with a leaf and its neighbor $v$.  
2. Remove $v$ to get a forest.  
   - If all components have size $< \frac{n}{3}$, combine some to get a forest that is $\leq \frac{2n}{3}$ and $\geq \frac{n}{3}$  , and the rest will also be $<\frac{2n}{3}$  .  
   - If there is a large component $T_1$, recurse:  
     - Take the neighbor $v_1$ of $v$ inside $T_1$.  
     - Remove $v_1$ and check the new components.  
     - Eventually, we find a split where both parts have $\leq \frac{2n}{3}$ vertices.  
