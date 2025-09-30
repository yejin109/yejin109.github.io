---
title: "[LLM-RL] Lecture 2: Value Functions"
toc: true
use_math: true
categories:
- LLM-RL
tags:
- Large Language Model
- Reinforcement Learning
---

**Overview.**  
This post focuses on **Value Functions** in reinforcement learning.  
We begin with a quick recap of the RL setup, then introduce **state-value / action-value functions**,  
explain the **Bellman Equation**, and finally discuss **Optimal Value Functions**.  

---

## 1. Recap: RL Scenario

Reinforcement Learning is built on the interaction between **agent** and **environment**:

- **Agent (policy):** decides which action to take in a given state  
- **Environment (model):** returns the next state and reward  

Two main task types:  
- **Episodic tasks:** with a natural termination point (e.g., a single game episode)  
- **Continual tasks:** with no fixed ending (e.g., robot control, ongoing service management)  

---

## 2. Value Functions

Given a policy $\pi$, value functions quantify how “good” a state or state-action pair is.

**State-value function**
$$
V^{\pi}(s) = \mathbb{E}_\pi \Big[ \sum_{t=0}^\infty \gamma^t r(s_t,a_t) \;\big|\; s_0 = s \Big].
$$

**Action-value (Q) function**
$$
Q^{\pi}(s,a) = \mathbb{E}_\pi \Big[ \sum_{t=0}^\infty \gamma^t r(s_t,a_t) \;\big|\; s_0=s, a_0=a \Big].
$$

*Intuition:*  
- $V^\pi(s)$: “How good is it to be in state $s$?”  
- $Q^\pi(s,a)$: “How good is it to take action $a$ in state $s$?”  

---

## 3. Bellman Equation

The value functions can be defined **recursively** using the Bellman Equation.

**State-value Bellman Equation**
$$
V^\pi(s) = \sum_a \pi(a|s)\Big[r(s,a) + \gamma \sum_{s'} P(s'|s,a) V^\pi(s')\Big].
$$

**Action-value Bellman Equation**
$$
Q^\pi(s,a) = r(s,a) + \gamma \sum_{s'} P(s'|s,a)\sum_{a'} \pi(a'|s') Q^\pi(s',a').
$$

*Key idea:*  
The current value = **immediate reward + discounted next value**.  
Value functions are the **unique solution** to the Bellman equations.

---

## 4. Optimal Value Functions

For the optimal policy $\pi^*$:

- **Optimal state value**
$$
V^*(s) = \max_\pi V^\pi(s)
$$

- **Optimal action value**
$$
Q^*(s,a) = \max_\pi Q^\pi(s,a)
$$

They satisfy the **Bellman Optimality Equations**:

- State-value
$$
V^*(s) = \max_a \Big[ r(s,a) + \gamma \sum_{s'} P(s'|s,a) V^*(s') \Big]
$$

- Action-value
$$
Q^*(s,a) = r(s,a) + \gamma \sum_{s'} P(s'|s,a)\max_{a'} Q^*(s',a')
$$

---

## 5. Example (Gridworld)

A simple **Gridworld** example:

- State $A \to A’$: reward 10  
- State $B \to B’$: reward 5  
- Other moves: reward 0  
- Stay in place: reward -1  

By computing $V^\pi$ and $Q^\pi$, we can see how the Bellman Equation captures  
the recursive structure of rewards and transitions.

---

## ✨ Summary

- Value functions quantify the **long-term performance** of a policy.  
- The Bellman Equation formalizes “current value = immediate reward + discounted future value.”  
- Optimal value functions $V^*$ and $Q^*$ define the foundation for finding **optimal policies**.  

