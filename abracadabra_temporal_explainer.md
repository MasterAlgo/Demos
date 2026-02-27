# Abracadabra — Predictive Coding Demo

## The Setup

This demo extends the [spatial pattern detector](abracadabra.html) into the temporal domain — and adds one key idea: **prediction from partial evidence**.

The target sequence "ABRACADABRA" (11 symbols, 5 unique letters, ~48K possible input states) is presented one symbol at a time, stride by stride. A sliding sensor window of adjustable width moves across the stream. A set of latent AND-nodes — each tuned to a specific consecutive subpattern of the target — fire when the sensor sweeps past their pattern. They then sustain their activation for N strides, staying "alive" while the window moves on.

At the top sits an inference node. But unlike the spatial demo's hard AND, this one fires when **K out of N** latent nodes are simultaneously active. K is a parameter.

That single change — from "all must fire" to "K must fire" — is the difference between recognition and **prediction**.

---

## What K Controls

**K = N (all nodes required):**
The inference node waits for every subpattern to confirm. This is sustained full recognition — reliable, but conservative. The network doesn't commit until complete evidence is in.

**K < N (partial evidence threshold):**
The inference node fires as soon as K subpatterns have confirmed, while the remaining ones are still incoming. On a **sparse natural manifold**, this is safe — the probability that any random sequence produces K matching subpatterns in sequence is vanishingly small. Partial evidence is already decisive.

This early firing is predictive coding: the system completes "abracada..." before "bra" arrives.

---

## The False Positive Experiment

Try this sequence:

1. **Fanin=2, K=1** → false positives appear immediately. A 2-symbol subpattern is a weak constraint, and requiring only 1 match means almost any random sequence triggers inference. The input space is too dense relative to the detector's selectivity.

2. **Fanin=5, K=2** → false positives drop sharply. Each latent node now checks 5 specific symbols in sequence — much harder to satisfy by accident. Two such nodes firing simultaneously is already very strong evidence.

3. **Fanin=5, K=3** → near zero false positives, but inference fires earlier than K=N because sustain bridges the gap between the first and last subpattern matches.

This is the core demonstration: **sparsity of the natural manifold enables early, reliable prediction**. The network doesn't need to be conservative (K=N) because the input space it operates in is not uniform — real patterns are rare, and partial matches are highly diagnostic.

---

## Sustain as Working Memory

When a latent node fires and sustains, it's holding a trace — "I saw ABRAC a few strides ago." That trace needs to stay alive long enough to overlap with later subpattern matches.

Too short sustain → early matches fade before late ones confirm → false negatives. Too long sustain → temporally distant coincidences in random sequences accidentally overlap → false positives.

The sweet spot is determined by the **temporal spread** of the subpatterns across the sequence. This is directly analogous to working memory in biological systems — the duration over which a higher-level representation stays active while awaiting confirming evidence.

At higher cortical layers, sustain gets longer. A concept activated by early evidence stays "in play" for hundreds of milliseconds. Pathologically long sustain is the predictive coding account of obsession and hallucination — a prior so strongly held that contradictory bottom-up evidence can't override it.

---

## Connection to Biology

Each latent node approximates a biological coincidence-detector neuron — one that fires when a specific sparse pattern of inputs co-activates. The sustain approximates the maintained spike train or synaptic trace that keeps the signal alive after the trigger.

The K-of-N inference threshold approximates a **leaky integrate-and-fire** neuron accumulating charge from multiple inputs — firing when enough sustained inputs push it over threshold, not necessarily all of them.

The result is a minimal but honest toy model of how cortical hierarchies do temporal pattern recognition: local subpattern detectors at lower layers, sustained traces bridging time, partial-evidence integration at higher layers, and early prediction when the natural manifold is sparse enough to make partial matches diagnostic.

---

## Try This

- **K=N, sustain=7** — baseline sustained recognition, note avg fire stride
- **K=N-1, sustain=7** — one node can be missing, watch prediction fire earlier
- **Fanin=2, K=1** — watch false positives flood in, sparsity assumption broken
- **Fanin=6, K=2** — strong prediction with very low false positive rate
- **Reduce sustain below gap between first and last subpattern** — false negatives appear

---

*Part of the Neural Pattern Lab — interactive explorations of biological neural computation.*
*Previous demos: [Abracadabra Spatial](abracadabra.html) · [LIF Neuron](index_lif.html)*
