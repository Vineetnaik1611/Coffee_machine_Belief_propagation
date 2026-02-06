# Coffee Machine Fault Diagnosis using Belief Propagation

This project implements a **probabilistic fault-diagnosis system** for an espresso coffee machine using **graphical models** and **belief propagation**.  
Given partial observations (e.g. lights on, pump sound, espresso quality), the model infers the **most likely underlying component failure**.

The goal is to demonstrate how **system-level reasoning** can be applied to diagnose faults in complex machines where failures are hidden and symptoms are noisy.

---

## 🧠 Key Idea

Instead of hard-coded rules or black-box machine learning models, this project uses:

- **Bayesian Networks** to model cause-effect relationships
- **Factor Graphs** to enable efficient inference
- **Belief Propagation** to compute marginal probabilities of failures

This approach is well-suited for:
- Hardware diagnostics
- Robotics systems
- Industrial machines
- Scenarios with incomplete or uncertain observations

---

## 🔧 Coffee Machine Model

The coffee machine is modeled as a set of **binary random variables**, grouped into three categories:

### 1. Failures (Hidden Root Causes)
These represent components that can be broken:
- `he` — No electricity
- `fp` — Fried power supply
- `fc` — Fried circuit board
- `wr` — Water reservoir empty
- `gs` — Group head gasket broken
- `dp` — Dead pump
- `fh` — Fried heating element

### 2. Mechanisms (Hidden Internal States)
These represent internal system behavior:
- `pw` — Power supply works
- `cb` — Circuit board works
- `gw` — Water flows through group head

### 3. Diagnostics (Observable Symptoms)
These are observable tests or outcomes:
- `ls` — Room lights on
- `vp` — Voltage measured
- `lo` — Power light on
- `wv` — Water visible
- `hp` — Pump audible
- `me` — Espresso made
- `ta` — Espresso hot and tasty

---

## 📊 Data

- Each row represents a fully observed coffee machine
- All variables are binary (`0 = False`, `1 = True`)
- The dataset is roughly balanced between working and faulty machines
- Probabilities are learned from data using **Bayesian estimation**

---

## 🧮 Learning the Probabilities

Conditional probability distributions are learned using:

- **Beta(1,1)** priors (uniform prior)
- Maximum A Posteriori (MAP) estimation
- Conjugate Bayesian updating for Bernoulli variables

This ensures:
- No zero-probability issues
- Robustness to noisy or limited data

---

## 🔄 From Bayesian Network to Factor Graph

Belief propagation is easier to implement on a **factor graph** than directly on a Bayesian network.

The process:
1. Each random variable becomes a node
2. Each conditional probability becomes a factor node
3. Edges represent dependencies between variables and factors

This structure enables efficient message passing.

---

## 📡 Belief Propagation

Belief propagation works by:
- Passing messages between random variables and factors
- Combining evidence from multiple sources
- Marginalizing over hidden variables

The output is a **posterior probability distribution** for every variable.

Observed variables are clamped to their known values during inference.

---

## 🔍 Fault Diagnosis

To diagnose a machine:
1. Provide observed symptoms (e.g. pump sound, voltage present)
2. Run belief propagation
3. Rank **failure variables only**
4. Select the failure with the highest probability

Diagnostics are treated as **evidence**, not failures.

---

## ✅ Example Output

```text
Observed: {pump audible = False, espresso tasty = False}

Most probable broken part:
→ Dead pump (dp)
