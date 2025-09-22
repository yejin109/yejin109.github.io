---
title: "[LLM-RL] Lecture 1: MDP, Objective, Value Functions, and Imitation Learning"
toc: true
use_math: true
categories:
- LLM-RL
tags:
- Large Language Model
- Reinforcement Learning
---

**Overview.** This post builds from the **MDP framework** to the **RL objective** and **value functions**, then contrasts pure RL with **Imitation Learning** (IL), focusing on **Behavior Cloning (BC)** and **DAgger**. The dependency chain is  
- **MDP → RL Objective → Value/Action-Value → Imitation Learning (BC, DAgger)**.

---

## 1. Markov Decision Process (MDP)

**Definition.** An MDP formalizes sequential decision-making under uncertainty as a 5-tuple:
$$
\mathcal{M} = (\mathcal{S},\, \mathcal{A},\, P,\, r,\, \gamma).
$$

- $\mathcal{S}$: state space  
- $\mathcal{A}$: action space  
- $P(s'\mid s,a)$: transition kernel (probability of next state $s'$ given current $(s,a)$)  
- $r(s,a) \in \mathbb{R}$: reward function  
- $\gamma \in [0,1)$: discount factor

**Markov property.**
$$
P(s_{t+1}\mid s_t,a_t, s_{t-1},a_{t-1},\ldots) \,=\, P(s_{t+1}\mid s_t,a_t).
$$
*Intuition.* The future only depends on the *current* state-action pair—no hidden memory beyond the present.

**Trajectory and return.** A trajectory is $(s_0,a_0,s_1,a_1,\ldots)$ with return
$$
G_t \,=\, \sum_{k=0}^{\infty} \gamma^k \, r(s_{t+k}, a_{t+k}).
$$

---

## 2. Objective in Reinforcement Learning

A **policy** $\pi(a\mid s)$ maps states to (possibly stochastic) actions. The standard RL objective is the expected discounted return:
$$
J(\pi) \,=\, \mathbb{E}_{\pi} \Bigg[ \sum_{t=0}^{\infty} \gamma^t \, r(s_t, a_t) \Bigg].
$$

*Physical meaning.* $J(\pi)$ measures the **long-run performance** of $\pi$: larger when the policy accrues higher rewards sooner (due to discounting).

---

## 3. Value Functions (State-Value, Action-Value)

These quantify how **good** states or state-action pairs are under a policy.

**State-value function.**
$$
V^{\pi}(s) \,=\, \mathbb{E}_{\pi} \Bigg[ \sum_{t=0}^{\infty} \gamma^t \, r(s_t,a_t) \;\Big|\; s_0=s \Bigg].
$$

**Action-value (Q) function.**
$$
Q^{\pi}(s,a) \,=\, \mathbb{E}_{\pi} \Bigg[ \sum_{t=0}^{\infty} \gamma^t \, r(s_t,a_t) \;\Big|\; s_0=s,\, a_0=a \Bigg].
$$

*Intuition.*
- $V^{\pi}(s)$: “How good is it to **be in** state $s$ under $\pi$?”  
- $Q^{\pi}(s,a)$: “How good is it to **take action $a$ in $s$**, then follow $\pi$?”

**Bellman expectation equations.**
$$
\begin{aligned}
V^{\pi}(s) \,&=\, \sum_{a} \pi(a\mid s) \Big[ r(s,a) \, + \, \gamma \sum_{s'} P(s'\mid s,a) \, V^{\pi}(s') \Big],\\
Q^{\pi}(s,a) \,&=\, r(s,a) \, + \, \gamma \sum_{s'} P(s'\mid s,a) \, \sum_{a'} \pi(a'\mid s') \, Q^{\pi}(s',a').
\end{aligned}
$$

*Physical intuition.* The value equals **immediate reward + discounted value of what follows**, averaged over policy and dynamics.

> **(Scope note)** We stop at definitions. Optimality equations, policy improvement, and control (e.g., $V^*, Q^*$) are intentionally omitted per your request.

---

## 4. Imitation Learning (IL)

IL bypasses direct reward optimization by **learning from expert demonstrations**.

### 4.1 Behavior Cloning (BC)

**Setup.** Given a dataset of expert pairs
$$
\mathcal{D} \,=\, \{(s_i, a_i)\}_{i=1}^{N},
$$
learn $\pi_\theta(a\mid s)$ via supervised learning.

**Objective (examples).**
- **Discrete actions (cross-entropy):**
  $$
  \min_{\theta} \; \mathcal{L}(\theta) \,=\, -\frac{1}{N}\sum_{i=1}^{N} \log \pi_\theta(a_i\mid s_i).
  $$
- **Continuous actions (squared error on mean policy):**
  $$
  \min_{\theta} \; \mathcal{L}(\theta) \,=\, \frac{1}{N}\sum_{i=1}^{N} \lVert a_i - \mu_\theta(s_i) \rVert^2.
  $$

**Key limitation: compounding errors.** Small mistakes take the agent to **off-distribution** states (unseen during training), where errors can snowball.

### 4.2 DAgger (Dataset Aggregation)

**Goal.** Reduce distribution shift by **iteratively** collecting data from the learner’s own state distribution.

**Algorithm (high-level).**
1. **Initialize** $\pi_1$ with BC on expert data $\mathcal{D}_0$.
2. For $k=1,2,\ldots,K$:
   - Deploy a **mixture** policy $\pi_k$ to roll out and collect states visited by the learner.
   - Query the **expert** for the *correct* action on those states to obtain labeled pairs.
   - **Aggregate** datasets: $\mathcal{D} \leftarrow \mathcal{D} \cup \{(s,a^{\text{expert}})\}$.
   - **Retrain** to get $\pi_{k+1}$ on the aggregated $\mathcal{D}$.

**Why it helps.** The training set progressively matches the **state distribution induced by the learned policy**, closing the covariate shift gap that hurts BC.

> **Note.** We intentionally exclude Inverse RL per your scope.

---

## References / Further Study

- 🎥 **Ernest Ryu 교수님 강의 (YouTube)**  
  <https://www.youtube.com/watch?v=R2oT9Tcv0eU&list=PLir0BWtR5vRp5dqaouyMU-oTSzaU5LK9r&index=2>
- 📄 **Imitation Learning 정리 (BC, DAgger)**  
  <https://jhrobotics.tistory.com/37>
- 📄 **MDP 정리 블로그 1**  
  <https://techblog-history-younghunjo1.tistory.com/553>
- 📄 **MDP 정리 블로그 2**  
  <https://untitledtblog.tistory.com/139>
- 📑 **이영운 교수님 강의안** (첨부파일 참고)
