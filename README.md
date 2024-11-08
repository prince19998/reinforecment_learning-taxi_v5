# Reinforecment learning taxi

This environment is part of the Toy Text environments. Please read that page first for general information.

Action Space

Discrete(6)

Observation Space

Discrete(500)

Import

gym.make("Taxi-v3")

The Taxi Problem from “Hierarchical Reinforcement Learning with the MAXQ Value Function Decomposition” by Tom Dietterich

Description
There are four designated locations in the grid world indicated by R(ed), G(reen), Y(ellow), and B(lue). When the episode starts, the taxi starts off at a random square and the passenger is at a random location. The taxi drives to the passenger’s location, picks up the passenger, drives to the passenger’s destination (another one of the four specified locations), and then drops off the passenger. Once the passenger is dropped off, the episode ends.

Map:

+---------+
|R: | : :G|
| : | : : |
| : : : : |
| | : | : |
|Y| : |B: |
+---------+
Actions
There are 6 discrete deterministic actions:

0: move south

1: move north

2: move east

3: move west

4: pickup passenger

5: drop off passenger

Observations
There are 500 discrete states since there are 25 taxi positions, 5 possible locations of the passenger (including the case when the passenger is in the taxi), and 4 destination locations.

Note that there are 400 states that can actually be reached during an episode. The missing states correspond to situations in which the passenger is at the same location as their destination, as this typically signals the end of an episode. Four additional states can be observed right after a successful episodes, when both the passenger and the taxi are at the destination. This gives a total of 404 reachable discrete states.

Each state space is represented by the tuple: (taxi_row, taxi_col, passenger_location, destination)

An observation is an integer that encodes the corresponding state. The state tuple can then be decoded with the “decode” method.

Passenger locations:

0: R(ed)

1: G(reen)

2: Y(ellow)

3: B(lue)

4: in taxi

Destinations:

0: R(ed)

1: G(reen)

2: Y(ellow)

3: B(lue)

Info
step and reset(return_info=True) will return an info dictionary that contains “p” and “action_mask” containing the probability that the state is taken and a mask of what actions will result in a change of state to speed up training.

As Taxi’s initial state is a stochastic, the “p” key represents the probability of the transition however this value is currently bugged being 1.0, this will be fixed soon. As the steps are deterministic, “p” represents the probability of the transition which is always 1.0

For some cases, taking an action will have no effect on the state of the agent. In v0.25.0, info["action_mask"] contains a np.ndarray for each of the action specifying if the action will change the state.

To sample a modifying action, use action = env.action_space.sample(info["action_mask"]) Or with a Q-value based algorithm action = np.argmax(q_values[obs, np.where(info["action_mask"] == 1)[0]]).

Rewards
-1 per step unless other reward is triggered.

+20 delivering passenger.

-10 executing “pickup” and “drop-off” actions illegally.

Arguments
gym.make('Taxi-v3')
Version History
v3: Map Correction + Cleaner Domain Description, v0.25.0 action masking added to the reset and step information

v2: Disallow Taxi start location = goal location, Update Taxi observations in the rollout, Update Taxi reward threshold.

v1: Remove (3,2) from locs, add passidx<4 check

v0: Initial versions release
