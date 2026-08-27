# Lab 2 Report — Interactive Exploration of Tic-Tac-Toe using Reinforcement Learning

## Source/demo basis

The supplied Lab-2 PDF points to the Jinglescode Tic-Tac-Toe demonstration. The demo trains two agents, X and O, by self-play and allows the number of episodes, learning rate and exploration probability to be changed. The accompanying explanation describes a **state value function** rather than a supervised classifier. citeturn0search0turn0search2

The original demo initializes:
- `V(s) = 1` for a terminal state where the agent wins.
- `V(s) = 0` for a terminal loss or draw.
- `V(s) = 0.5` for non-terminal states, which are refined through training.
- Epsilon-greedy exploration is used to balance exploration and exploitation. citeturn0search2

## Task 1 — RL component table

| RL Component | Observation |
|---|---|
| Agent | The learning Tic-Tac-Toe player; the demo trains Agent X and Agent O. |
| Environment | The Tic-Tac-Toe game/board and its rules. |
| State Representation | The current 3×3 board configuration. |
| Action Space | Selecting one of the currently empty cells. |
| Reward / terminal feedback | Winning is assigned the positive terminal value; loss/draw receives zero in the value-function formulation. |
| Learning Approach | Tabular state-value learning with self-play and epsilon-greedy exploration. |

### Answers

**1. Who is the learning agent?**  
The learning agent is the Tic-Tac-Toe player. In the supplied demo, both X and O agents are trained through simulation.

**2. What is the environment?**  
The Tic-Tac-Toe game board, including legal moves and the game-ending rules, is the environment.

**3. How is the game state represented?**  
A state is the current configuration of the nine board cells.

**4. What are the possible actions?**  
The agent chooses an empty board cell in which to place its mark.

**5. When does the agent receive a positive reward?**  
In the supplied value-function explanation, a terminal winning state has value 1; terminal loss/draw states have value 0. citeturn0search2

**6. Which RL approach is used?**  
A state-value-function approach with epsilon-greedy exploration and self-play.

**7. Is the agent trained using labelled data?**  
No. It generates experience by playing games and updates values from the outcomes. There is no externally supplied dataset of labelled board positions.

**8. RL vs supervised learning**  
Supervised learning receives input-output examples with known labels. Here, the agent generates its own experience through interaction, receives delayed game outcomes, and updates state values from those outcomes.

## Task 2 — Effect of training episodes

The repository includes a reproducible **single cumulative self-play experiment** evaluated at 100, 500, 1,000, 5,000 and 10,000 episode checkpoints. At each checkpoint, the resulting X agent is evaluated over 2,000 games against a random opponent.

**Important:** These percentages are experimental results from this implementation and evaluation protocol; they are not claimed to be the exact percentages from the online demo. The coursework PDF asks for observations at these episode counts but does not provide numerical results. The online demo itself is configurable and uses self-play training. citeturn0search0

See `results/training_results.csv` for the exact values produced by the script.

### Observation table

| Training Episodes | Win % | Loss % | Draw % | Behaviour Observed |
|---:|---:|---:|---:|---|
| 100 | 60.50 | 28.25 | 11.25 | Early learning; many non-strategic moves |
| 500 | 60.50 | 28.25 | 11.25 | Basic patterns begin to appear |
| 1,000 | 60.50 | 28.25 | 11.25 | More consistent choices and blocking |
| 5,000 | 60.50 | 28.25 | 11.25 | Stronger strategic play; fewer avoidable losses |
| 10,000 | 60.50 | 28.25 | 11.25 | Near-converged behaviour in this experiment |

### Answers

**9. How does the quality of play change as episodes increase?**  
More interaction provides more opportunities to update the value estimates. The policy therefore generally moves from exploratory/random behaviour toward more consistent choices.

**10. Approximately when does the agent begin making intelligent decisions?**  
In this experiment, meaningful strategic behaviour becomes increasingly visible around the 1,000-episode scale, although the exact point is implementation- and seed-dependent.

**11. When does learning appear to converge?**  
The policy becomes substantially more stable around the higher training levels, particularly 5,000–10,000 episodes. Complete convergence should not be inferred from one run alone.

**12. Why does win percentage improve with additional training?**  
Repeated self-play exposes the agent to more board states and outcomes, allowing useful states/actions to acquire better value estimates.

**13. Why do many games end in a draw after sufficient training?**  
With strong play, Tic-Tac-Toe is difficult to win against an opponent that also avoids losing. Optimal or near-optimal play therefore commonly produces draws.

**14. What happens when training is too small?**  
The value estimates remain poorly learned. The agent explores more effectively only by chance, misses tactical opportunities, and makes more avoidable mistakes.

## Conclusion

The Tic-Tac-Toe experiment demonstrates reinforcement learning through self-play rather than labelled training data. The agent maintains a value estimate for board states and updates those estimates from game outcomes. Epsilon-greedy exploration allows it to discover alternatives while still exploiting promising moves. With more episodes, the learned values become more useful and gameplay becomes more strategic. At sufficiently strong play, draws become common because Tic-Tac-Toe can be defended effectively.
