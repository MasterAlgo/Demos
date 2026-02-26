# Abracadabra Pattern Detector — What This Demo Shows

## The Setup

Imagine an 11-node input layer where each node can hold one of 5 discrete signal grades. The target sequence "ABRACADABRA" has exactly 11 symbols and 5 unique letters — a perfect fit. The full input space has 5^11 ≈ 48,000 possible states.

Above the input layer sits a small **latent layer** of hard AND-nodes. Each latent node is wired to a specific subset of input positions and fires only if *all* of its required positions match a specific pattern — like the substring "ABRAC" at positions 0–4, or "CADA" at positions 4–7. This is a deliberate simplification of biological coincidence-detector neurons, which integrate thousands of synaptic inputs and fire when a specific correlated subset activates together.

At the top sits a single **inference node** — a hard AND over all latent nodes. It fires only when every latent node fires simultaneously.

---

## The Key Question

*How many false positives does this network produce when fed random patterns?*

A false positive means: a random input (not "ABRACADABRA") causes the inference node to fire anyway — a spurious recognition.

---

## What the Demo Shows

**With overlapping subpattern detectors** (e.g. "ABRAC", "ACADA", "DABRA" sharing input positions):

Each latent node is already a tight constraint — it requires *N* specific positions to match simultaneously. The probability of a random input satisfying one such node drops as 1/5^N. With overlapping nodes, their constraints are correlated and jointly carve out a very small region of the 48K input space. The result: **false positives drop to near zero**, even with just 3 nodes of fanin 5.

**With non-overlapping detectors** (disjoint input patches, low fanin):

Each node examines a small independent slice. Low fanin means weak selectivity — a large fraction of random inputs will satisfy any given 2-input AND gate. Since the nodes are independent, there's no "joint tightening" effect. **False positives accumulate.**

---

## The Deeper Point

Natural signals — speech, text, images — don't fill their theoretical input space uniformly. They occupy a tiny, structured *manifold*. "ABRACADABRA" and patterns like it are rare in the space of all 48K states.

This sparsity means that **overlapping local detectors are sufficient for unique identification**. You don't need to examine every input position exhaustively. A few well-placed overlapping subpattern windows, with enough fanin, lock onto the target manifold and reject everything else.

This is why biological sensory hierarchies — auditory cortex, visual cortex, language areas — are organized as overlapping receptive fields at each layer. The overlap isn't inefficiency. It's the mechanism that creates tight joint constraints with minimal neurons.

---

## Parameters to Explore

- **Latent Nodes**: more nodes = more constraints = fewer false positives (up to a point)
- **Fanin (legs)**: higher fanin = exponentially tighter per-node selectivity
- **Overlap on/off**: the most dramatic switch — observe the false positive rate change
- **Rewire**: same parameters, different random wiring — shows variance across architectures
- **Series Length / Speed**: run longer series for statistically meaningful false positive rates

---

## Connection to Biology

The hard AND-node is a deliberate simplification of a **Leaky Integrate-and-Fire neuron** with ~10,000 synaptic inputs, ~200 of which need to fire in correlation to push the neuron over threshold. The combinatorial space of possible coincidence patterns (C(10000, 200)) is astronomically large — which is what gives biological neural tissue its expressive power.

The AND approximation preserves the essential logic: a neuron is a **coincidence detector** for a specific sparse input pattern. Everything else is implementation detail.

When partial subpattern matches accumulate enough evidence to trigger the inference node *before* the full pattern is presented — that's **predictive coding** in its simplest form. The network doesn't wait for complete input; it fires when the posterior probability of the target is already overwhelming, because the natural manifold is sparse enough that partial matches are highly diagnostic.

---

*Part of the Neural Pattern Lab — interactive explorations of biological neural computation.*
