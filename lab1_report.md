# Lab 1 Report — Exploring Reinforcement Learning Environments using Gymnasium

## Task 1 — Environment setup

The required Python libraries are Gymnasium, NumPy and Matplotlib. The program prints the installed Gymnasium version to verify the installation.

## Task 2 — Create and initialize CartPole-v1

`CartPole-v1` is created with:

```python
env = gym.make("CartPole-v1")
observation, info = env.reset(seed=42)
```

The reset operation returns the initial observation and an information dictionary.

## Task 3 — Observation and action spaces

For `CartPole-v1`:

- **Observation space:** `Box` with 4 continuous values.
- **Observation:** cart position, cart velocity, pole angle and pole angular velocity.
- **Action space:** `Discrete(2)`.
- **Possible actions:** 0 or 1, corresponding to applying force to the left or right.

The exact numerical initial observation can vary with the environment/library version and seed.

## Task 4 — Random agent

At every time step the program samples:

```python
action = env.action_space.sample()
```

and passes the action to `env.step(action)`.

The program records step number, action, observation, reward and termination status, then reports the number of steps and cumulative reward.

## Expected learning outcome

This experiment demonstrates the RL interaction loop:

**state/observation → action → environment → reward + next observation**

A random agent does not learn a policy, so its performance is generally unstable. The experiment is intended to build familiarity with RL environments, spaces, actions, rewards and episode termination.

## Submission evidence

Run `lab1_cartpole.py` and capture the terminal output and plot if your instructor specifically requires live execution screenshots.
