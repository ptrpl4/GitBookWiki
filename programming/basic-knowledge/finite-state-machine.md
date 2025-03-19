# FSM

A model that can be in one of a finite number of states at any given time. It moves from one state to another based on input data or events.

### Key Components of a State Machine:

- **States**: These are the distinct conditions or situations in which the system can exist. Each state represents a specific configuration of the system.
- **Transitions**: These are the rules or conditions that dictate how the system moves from one state to another. Transitions are usually triggered by events or inputs.
- **Events/Inputs**: These are external factors or signals that can cause a state transition. They can be user actions, messages, or other stimuli.
- **Initial State**: This is the state in which the system starts when it is initialized.
- **Final States**: These are states that signify the completion of a process or the end of the state machine's operation.
### Types of State Machines:

- **Deterministic Finite State Machine (DFSM)**: In a DFSM, for each state and input, there is exactly one transition to a next state. This means the behavior of the machine is predictable.
- **Nondeterministic Finite State Machine (NFSM)**: In an NFSM, for a given state and input, there can be multiple possible next states. This introduces ambiguity in the transitions.