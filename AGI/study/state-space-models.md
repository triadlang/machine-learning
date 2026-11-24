# Interface: structured state space models

This is the mathematically tightest of the six cross-domain mappings. The auxiliary-field memory equation derived in is identical, term by term, to the hidden-state update of the structured state space model architectures that have become a leading approach to sequence modeling in machine learning since 2021.

## The equivalence

The defining update equation of structured state space models (Gu, Goel & Ré 2021; Smith, Warrington & Linderman 2023; Gu & Dao 2024; Peng et al. 2023) is:

$$
\partial_t \mathbf{h}(t) = \mathbf{A}\,\mathbf{h}(t) + \mathbf{B}\,u(t),
$$

with $\mathbf{h}(t) \in \mathbb{R}^d$ a hidden state, $u(t)$ an input signal, and $\mathbf{A}, \mathbf{B}$ structured matrices whose eigenvalues control the temporal range over which past inputs persist. For diagonal $\mathbf{A}$ with eigenvalues $-\nu_j$ (the architecture variant of S4D and the basic Mamba update), the system decouples into $d$ independent scalar equations:

$$
\partial_t h_j(t) = -\nu_j h_j(t) + b_j u(t).
$$

For $b_j = \nu_j$:

$$
\partial_t h_j = \nu_j u - \nu_j h_j = \nu_j (u - h_j).
$$

This is precisely the auxiliary-field equation of the memory-NLS, with the identification $h_j = y_j$ and $u = \rho$:

$$
\partial_t y_j = \nu_j(\rho - y_j).
$$

The auxiliary fields of the present equation and the hidden state components of a diagonal-state SSM are the same object, expressed in different physical and computational contexts.

## No calibration is required

This is the property that distinguishes this correspondence from the others in this folder. No dimensional identification, no choice of physical units, no scaling factor mediates between the two formulations. The two equations are literally the same equation. A physicist who writes the Markovian embedding of an integral memory potential and a machine learning researcher who writes the diagonal hidden-state update of a structured state space model are writing the same mathematical object.

The two communities arrived at this object by entirely different paths. Physicists arrived via Mori–Zwanzig projection-operator methods (Mori 1965; Zwanzig 1961), originally developed to reduce systems with many degrees of freedom to systems with few in classical statistical mechanics. Machine learning researchers arrived via the search for efficient alternatives to attention mechanisms in sequence models, motivated by the quadratic computational cost of attention and the difficulty of capturing very long-range dependencies. Neither community needed the other's motivation to reach the same mathematical structure.

This independent convergence on identical mathematical form is, on the structural-realist reading documented in , evidence that the form is recovering something invariant about the underlying class of systems. Two communities under different pressures with different goals reaching the same equation suggests the equation is the unique answer to a structural question both communities were asking, even if neither phrased it that way.

## What the present equation adds to the SSM baseline

The diagonal-state SSM update is the linear memory subsystem of the full equation. The full equation extends this baseline in four structurally distinct directions, each of which corresponds to an active research question in the machine learning literature.

### 1. Nonlinear self-interaction in the state

Standard SSMs are linear in the hidden state $\mathbf{h}$. The inputs $u$ may be projected through learned mixing matrices, and "selective" variants (Mamba; Gu & Dao 2024) make $\mathbf{A}$ and $\mathbf{B}$ input-dependent, but the state dynamics remain linear in $\mathbf{h}$ itself. The present equation introduces a cubic nonlinearity $\Lambda |\Psi|^2$ in the wave equation that drives $\rho = |\Psi|^2$ and hence the auxiliary fields. The nonlinearity acts at the level of the variable that produces the input to the SSM-like memory.

Nonlinear state space models are an active and incomplete extension within the ML literature. Work in this direction includes neural ordinary differential equations (Chen et al. 2018), Hyena (Massaroli et al. 2023), and others. The present equation provides a specific physically-motivated form of nonlinearity, the L²-norm-conserving cubic NLS interaction, that has well-studied properties from the physics literature and that produces, in particular, the anti-collapse phenomenology documented in .

### 2. Anti-collapse via temporal memory lag

Representation collapse, the convergence of distinct inputs onto degenerate output representations, is a documented failure mode of contemporary neural architectures. Self-supervised methods such as SimSiam and BYOL (Chen & He 2021; Grill et al. 2020) prevent collapse via architectural asymmetries: stop-gradient operations, predictor networks, exponential moving averages of weights. The theoretical basis of these prescriptions is incomplete; they are known to work empirically but the precise mechanism by which they avert collapse remains a subject of investigation.

A second form of collapse, attention rank collapse, occurs in transformer architectures and is characterized by the attention matrix becoming low-rank with depth. Dong, Cordonnier & Loukas (2021) showed that pure-attention networks lose rank doubly exponentially with depth, and Noci et al. (2022) developed the theoretical framework. The architectural responses to this, skip connections, normalization layers, careful initialization, are also somewhat empirical.

The anti-collapse mechanism analyzed in  is structurally a candidate solution to both classes of failure mode. The mechanism is the delayed repulsive feedback from a temporal memory potential that lags the rising signal; the memory builds up while concentration is occurring, and releases at the moment the concentration would have locked in. The same mechanism, applied to neural representation dynamics, would provide a principled rather than ad-hoc resistance to representational collapse.

The dimensional rescaling discovered in , $\Sigma\lambda \sim |\Lambda|/d$, where $d$ is the dimensional concentration of the focal region, is the testable scaling prediction this mechanism makes. The corresponding ML hypothesis: the memory bandwidth required to stabilize representations scales with the effective rank of the attention or state representation.

### 3. Spontaneous discrete structure from continuous substrate

The Bravais lattice selection documented in  is, in machine-learning vocabulary, a model of emergent categorical structure from a continuous representational substrate. From an unstructured Gaussian initial state, the equation selects a specific discrete spatial symmetry (BCC in three dimensions, hexagonal in two) and selects it consistently across the relevant parameter range.

The grounding problem, how discrete symbolic categories arise from continuous neural representations, remains open in deep learning theory. The equation provides a concrete mechanism: a continuous, self-interacting field with appropriate temporal coupling spontaneously selects a discrete symmetry through modulational instability about the unperturbed uniform state. The selection is dynamical, not encoded; the symmetry is emergent, not specified.

Whether this mechanism scales from the field-theoretic setting to the very high-dimensional embedding spaces of neural networks is an empirical question for follow-up work. The structural fact, that a memory-augmented continuous field equation can produce discrete symmetry, is established in this work.

### 4. Fluctuation–dissipation-locked stochastic regularization

Noise injection during training is standard in deep learning, but the relationship between the magnitude of dissipative regularization (weight decay, attention dropout, etc.) and the magnitude of stochastic forcing (SGD noise, dropout, label noise) is typically tuned empirically. The fluctuation–dissipation theorem prescribes this relation exactly for thermodynamic systems; its analogue for optimization dynamics has been investigated recently (Yaida 2019; Liu et al. 2021), with suggestive evidence that principled noise schedules outperform heuristically tuned ones in the long-training regime.

The equation's noise term is FDT-locked by construction. Translated into optimization terms, the prescription would be: the stochastic forcing applied during training should be proportional to the square root of (dissipative regularization rate × bath temperature × step size). The temperature parameter is, in this translation, an analogue of the effective stochasticity of the input distribution. The prescription is parameter-poor, only $\gamma_0$ and $T$ are free, and removes the need to separately tune noise injection and weight decay.

## What this folder is and is not

This document is the most concrete of the cross-domain mappings because the mathematical correspondence is exact. It is also the most consequential for the work's potential reception in the machine learning community: a reader from machine learning who reads this document understands immediately that the work is not adjacent to their field but is, in a precise sense, a different formulation of objects they already work with.

This document is not a benchmark claim. The four extensions to the SSM baseline outlined above are structural claims about the equation; whether they translate into empirical performance improvements on standard sequence-modeling benchmarks (Long Range Arena, language modeling perplexity, etc.) is a question for the empirical work outlined in . That empirical work is the natural next direction.

## Common dismissals and why they do not apply

**"Trivial mathematical coincidence."** The auxiliary-field update $\partial_t y_j = \nu_j(\rho - y_j)$ and the diagonal SSM update $\partial_t \mathbf{h} = \mathbf{A}\mathbf{h} + \mathbf{B} u$ with $\mathbf{A}$ diagonal and eigenvalues $-\nu_j$ are not similar equations; they are the same equation written with different notation. Sections `## The equivalence` and `## No calibration is required` give the term-by-term identification. Calling a term-by-term identity a "coincidence" addresses the appearance of the equations rather than their content; the content is identical.

**"Different motivations, same equation by accident."** Two communities converging on the same mathematical object from different motivations is exactly the structural-realist signature. The physics community arrived at this equation by Mori–Zwanzig projection of an integro-differential field equation; the ML community arrived at it by search for efficient long-sequence representation. The convergence is informative because it is unmotivated by either community in terms of the other; if the mathematical form were arbitrary, this convergence would not have occurred.

**"ML does not need physics."** The work does not assert that ML needs physics. It asserts that an equation derived from physical structural axioms is, by mathematical identity, the same equation as the linear core of modern sequence models. Whether ML practitioners find this useful for their own work is for ML practitioners to decide. The structural fact, that the same equation arose independently, is established by the term-by-term identification and is not contingent on ML practitioners adopting the physics-side framing.


The original Mori–Zwanzig projection-operator references:

- Mori, H. (1965). Transport, collective motion, and Brownian motion. *Prog. Theor. Phys.* **33**, 423.
- Zwanzig, R. (1961). Memory effects in irreversible thermodynamics. *Phys. Rev.* **124**, 983.
