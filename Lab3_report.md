# Lab Assignment 3: GridWorld – Policy Evaluation and Value Iteration

## 1. Objective

This experiment formulates a deterministic GridWorld as a Markov Decision Process (MDP), implements iterative Policy Evaluation and Value Iteration, derives an optimal policy, and compares Random, Evaluated, and Optimal policies.

## 2. Environment

The GridWorld is a 4×4 grid:

```text
+-----+-----+-----+-----+
|  S  |     |     |     |
+-----+-----+-----+-----+
|     |  X  |     |     |
+-----+-----+-----+-----+
|     |     |     |     |
+-----+-----+-----+-----+
|     |     |     |  G  |
+-----+-----+-----+-----+
```

- Start state: `(0,0)`
- Goal state: `(3,3)`
- Obstacle: `(1,1)`
- Actions: Up, Down, Left, Right
- Normal movement reward: `-1`
- Goal reward: `+10`
- Discount factor: `γ = 0.9`
- Convergence threshold: `θ = 0.0001`

## 3. MDP Formulation

The MDP is:

`M = (S, A, P, R, γ)`

| Component | Definition |
|---|---|
| State Space | All non-obstacle grid cells |
| Action Space | Up, Down, Left, Right |
| Transition Dynamics | Deterministic |
| Reward Function | +10 at goal, otherwise -1 |
| Discount Factor | 0.9 |
| Terminal State | `(3,3)` |

The objective is to maximize discounted cumulative reward:

`G_t = R_(t+1) + γR_(t+2) + γ²R_(t+3) + ...`

## 4. Policy Evaluation

Policy Evaluation estimates the value of following a fixed policy `π`.

The Bellman expectation equation is:

`V^π(s) = Σ_a π(a|s) Σ_s' P(s'|s,a)[R(s,a,s') + γV^π(s')]`

For this deterministic environment and deterministic policy:

`V^π(s) = R(s,π(s),s') + γV^π(s')`

The implementation initializes all values to zero and repeatedly applies the Bellman update until the maximum change is below `0.0001`.

### Observation

The value function changes substantially during early iterations and then stabilizes. The reward from the terminal goal propagates backward through predecessor states. This demonstrates the central idea of dynamic programming in a known MDP.

## 5. Value Iteration

Value Iteration uses the Bellman optimality equation:

`V*(s) = max_a Σ_s' P(s'|s,a)[R(s,a,s') + γV*(s')]`

For the deterministic environment:

`V_(k+1)(s) = max_a [R(s,a,s') + γV_k(s')]`

After convergence, the optimal policy is obtained using:

`π*(s) = argmax_a [R(s,a,s') + γV*(s')]`

### Observation

Unlike Policy Evaluation, Value Iteration does not evaluate only one predetermined action. It considers every available action and retains the action with the highest expected return.

## 6. Policy Comparison

The notebook executes:

1. Random Policy
2. Evaluated Policy
3. Optimal Policy

For each policy, the following are recorded:

- Path
- Number of steps
- Cumulative reward
- Whether the goal was reached

The random policy is stochastic, so its result can differ between executions. A repeated 1000-run experiment is included to show its average behavior and success rate.

## 7. Policy Evaluation vs Value Iteration

| Aspect | Policy Evaluation | Value Iteration |
|---|---|---|
| Input | Fixed policy | MDP |
| Purpose | Evaluate a policy | Find optimal value |
| Bellman equation | Expectation | Optimality |
| Action selection | Determined by policy | Maximum over actions |
| Output | `V^π` | `V*` |
| Directly derives optimal policy | No | Yes |

The key distinction is:

> **Policy Evaluation asks: “How good is this policy?” Value Iteration asks: “What is the best achievable value?”**

## 8. Analysis Questions

### Q1. What is Policy Evaluation?

Policy Evaluation calculates the state-value function `V^π(s)` for a fixed policy.

### Q2. Why does the value function change over iterations?

Each Bellman update incorporates information from successor states. Therefore, reward information propagates through the state space over successive iterations.

### Q3. What is Value Iteration?

Value Iteration repeatedly applies the Bellman optimality equation to determine the optimal value function.

### Q4. Why does Value Iteration use `max()`?

Because the optimal value assumes the agent selects the action with the greatest expected return.

### Q5. Does Policy Evaluation find the optimal policy?

No. It evaluates the policy supplied to it. Policy improvement or Value Iteration is required to obtain an optimal policy.

### Q6. Why can a random policy take more steps?

The random policy does not know the long-term value of states. It can move away from the goal, revisit states, or remain in inefficient trajectories.

### Q7. Is the shortest geometric route always the optimal RL route?

No. RL optimises cumulative discounted reward. The reward function and discount factor determine the preferred trajectory.

### Q8. What happens when `γ = 0`?

Only the immediate reward is considered.

### Q9. What happens as `γ` approaches 1?

Future rewards become increasingly important.

### Q10. Why use a negative step reward?

It discourages unnecessary movement and encourages efficient goal reaching in this GridWorld.

## 9. Conclusion

This experiment demonstrated GridWorld as a Markov Decision Process and implemented Policy Evaluation and Value Iteration using the Bellman equations.

Policy Evaluation estimated the state-value function for a fixed policy and showed how reward information propagates through the environment until convergence. Value Iteration considered all possible actions and used the Bellman optimality equation to obtain the optimal value function and corresponding policy.

The policy execution experiment demonstrates that random decisions can produce inefficient trajectories or fail to reach the goal, while an optimal policy uses value information to select actions that maximize expected discounted return.

An important observation is that the shortest geometric path is not necessarily the optimal RL path in every problem. The reward structure and discount factor define what the agent actually optimizes.



## 11. References

- NeurOMatch Academy: https://deeplearning.neuromatch.io/tutorials/W3D4_BasicReinforcementLearning/student/W3D4_Tutorial1.html
- Shangtong Zhang, Reinforcement Learning implementations: https://github.com/ShangtongZhang/reinforcement-learning-an-introduction
- Sutton, R. S., & Barto, A. G., *Reinforcement Learning: An Introduction*, 2nd edition.
