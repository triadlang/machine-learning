# The six criteria

This document states the six criteria by which the work is evaluated and presents the self-assessment against each. The criteria are derived from the methodological frame in : the work makes a structural claim, the structural claim is evaluated by criteria appropriate to a structural theory, and the criteria are the minimum set sufficient to determine whether the structural form is internally consistent, externally reproducible, generatively productive, cross-domain coherent, parsimonious, and comprehensive.

Each criterion is stated as a structural requirement, then the work is assessed against it. The criteria themselves are derived from what a structural theory must satisfy in order to count as a structural theory at all; the prior-art credit (Worrall 1989; Ladyman & Ross 2007; Cartwright 1983; Lakatos 1970; Whitehead 1929) is acknowledged at the References block. The criteria do not rest on the authority of those authors.

## Criterion 1: Internal mathematical consistency

**Statement.** The theory must be mathematically consistent. In the numerical setting, the solver must conserve the quantities the theory says are conserved, to the precision the theory predicts. Failures of internal consistency are decisive: an internally inconsistent theory is mathematically defective regardless of its other virtues.

**Assessment for this work.** The solver in  passes the conservation diagnostics in  at the precision the theory predicts:

- Norm conservation under unitary dynamics: drift $< 10^{-13}$ in fp64 over 400 integration steps, consistent with machine precision.
- Pure dissipative decay: $\|\Psi\|^2(t)$ matches $e^{-2\gamma t}\|\Psi(0)\|^2$ to six significant figures, consistent with the analytical prediction.
- FDT thermalization: $\langle |\Psi|^2 \rangle$ per cell at stationary state matches $2T$ to within 0.5%, consistent with classical equipartition.
- Memory auxiliary field update: drift in the OU stationary state $< 10^{-13}$ over the integration window.

Each conservation diagnostic tests a different aspect of the implementation. The simultaneous passage of all four is non-trivial.

**Verdict.** The criterion is satisfied.

## Criterion 2: Reproducibility

**Statement.** All numerical results must be bit-for-bit reproducible from the published code with fixed random seeds. Reproducibility is the minimum requirement for any computational claim to be evaluated.

**Assessment for this work.** Every numerical result reported in  has a corresponding reproduction script in . The scripts use fixed random seeds and produce bit-for-bit identical output on identical hardware (NVIDIA RTX 4060 Laptop GPU, Arch Linux, CUDA 12.x). On different hardware, the outputs may differ at the level of floating-point rounding but the qualitative findings (orders of magnitude separations, Bravais selection, frequency ratios) are preserved.

The code is open-source and committed to this repository. The dependencies are documented in `requirements.txt` and `pyproject.toml`. The validation suite runs in approximately one minute on the reference hardware.

**Verdict.** The criterion is satisfied.

## Criterion 3: Generative scope

**Statement.** The theory must produce phenomena beyond those that are inputs. From minimal axioms, a non-trivial taxonomy of behaviors must follow. A theory that yields only what was put into it is not generative; a generative theory predicts more than its inputs.

**Assessment for this work.** From three structural axioms (P1, P2, P3) and the requirement that the equation be the most economical form consistent with all three, the work generates:

- Memory regularization of L²-critical NLS collapse (two dimensions).
- Memory regularization of L²-supercritical NLS collapse (three dimensions).
- Spontaneous emergence of crystalline spatial patterns from unstructured Gaussian initial states, with definite intrinsic wavelength.
- Spontaneous selection of body-centered cubic Bravais symmetry in three dimensions.
- Internal vibrational structure of the crystalline state with discrete modes.
- Broadband absorption of external periodic driving in the crystalline regime.
- Dimensional rescaling of the memory coupling required for anti-collapse: $\Sigma\lambda/|\Lambda| \sim 1/d$.
- Asymmetric roles of temporal and spatial non-locality in the memory kernel.
- Heterodyne mixing under bichromatic external driving.
- Exact mathematical correspondence with structured state space models in machine learning.

None of these is an axiom. Each follows from the dynamics of the equation that the axioms select.

**Verdict.** The criterion is satisfied. The generative scope is well in excess of what the inputs demanded.

## Criterion 4: Cross-domain coherence

**Statement.** The structural form of the theory must appear in independently documented domains. The appearance must be at the level of mathematical form and not merely at the level of metaphor. Cross-domain coherence is the principal test of structural realism: a structural theory that does not exhibit its form elsewhere is not structurally robust.

**Assessment for this work.** The six interfaces in .document the structural form's appearance in:

1. Other instances of nonlinear Schrödinger dynamics (BEC, optics, oceanography, plasma), mathematical form identical at the level of the bare NLS plus the natural domain-specific extensions.
2. Baryon acoustic oscillations in cosmological structure formation, structural form (wave propagation in a self-interacting coupled medium) identical at the relevant level of abstraction.
3. Chladni cymatic patterns, structural form (spontaneous geometric order from sustained oscillation in a self-coupled medium) identical.
4. Gamma-frequency neural entrainment, structural form (broadband absorption by a self-organized oscillating medium with memory hierarchy) identical, with one specific frequency in the band documented as biologically active.
5. Low-frequency acoustic resonance in megalithic chambers, under one specific dimensional identification, the equation's two principal frequency modes correspond to measured resonance bands.
6. Structured state space models, auxiliary-field equation is mathematically identical, term by term, to the diagonal SSM update.

The six mappings are independent: each is sourced from a separate body of peer-reviewed literature and does not depend on the others. The state space model correspondence is the mathematically exact one; the others vary in calibration sensitivity. The full set is the cross-domain evidence.

**Verdict.** The criterion is satisfied at strength varying across the six interfaces. The state space model interface alone would be sufficient to establish that the structural form appears elsewhere; the accumulation of the other five increases the structural coherence claim.

## Criterion 5: Parsimony

**Statement.** The number of independent axioms required must be small. The structure that follows must be the unique consequence of those axioms up to choices of coupling constants. A theory that requires many axioms to derive few consequences is non-parsimonious; a theory that requires few axioms to derive many consequences is parsimonious.

**Assessment for this work.** The work uses three structural axioms (P1, P2, P3). It derives one equation. From that equation it generates the ten phenomena listed under Criterion 3 and the six cross-domain correspondences listed under Criterion 4.

The ratio of derived consequences to input axioms is high. The number of input axioms (three) is at the lower end of what is typically required to specify a non-trivial physical theory.

**Verdict.** The criterion is satisfied.

## Criterion 6: Comprehensiveness

**Statement.** The theory must encompass, as limits and reductions, behaviors documented separately in established sub-fields. A theory that fails to recover known behaviors in the appropriate limits is incomplete.

**Assessment for this work.** The reductions in  tabulate the equations the work recovers:

- Free Schrödinger (no $\Lambda$, no memory, no $\Gamma$, no $\eta$).
- Gross–Pitaevskii (no memory, no $\Gamma$, no $\eta$).
- Sub-critical NLS (dispersive regime).
- Critical 2D NLS and supercritical 3D NLS (collapse regime).
- Lindblad-type open quantum system ($\gamma_0 > 0$, no $\eta$).
- Stochastic Gross–Pitaevskii (FDT-locked $\gamma_0$ and $\eta$).
- Fractional Schrödinger (non-trivial $\alpha$ and $\sigma$).
- Rashba spin–orbit Hamiltonian (spinor structure with gauge).
- Non-Hermitian skin Hamiltonian (spinor structure with momentum-dependent dissipation).
- Diagonal state space model (memory subsystem in isolation).

Each reduction has been independently developed in its corresponding sub-field. The present work recovers each in the appropriate limit and adds the structural extensions that the full equation includes.

**Verdict.** The criterion is satisfied.

## Cumulative assessment

The six criteria are satisfied. The work, on its own self-assessment, succeeds by the standards of a structural-realist evaluation. The reader is asked to perform their own assessment and compare; the criteria are stated above to make this comparison feasible.

The criteria are not the only possible standards for evaluating the work, but they are the standards appropriate to the methodological frame the work adopts. A reader who finds the methodological frame itself unconvincing, who insists on strict Popperian falsification of the global structural claim, will find the work's self-assessment unconvincing for reasons that are independent of the criteria. The disagreement at that level is methodological, not empirical, and is acknowledged in.

## Why these criteria and not others

The six criteria are not arbitrary. Each addresses a distinct structural property that a non-trivial theory must satisfy.

Internal mathematical consistency is non-negotiable; without it the theory is mathematically defective and no further evaluation matters. Reproducibility is the externalization of internal consistency: without it, the work cannot be evaluated by anyone other than its author. Generative scope distinguishes a productive theory from a stipulation: a stipulation predicts what it was told to predict; a generative theory predicts more than its inputs demand. Cross-domain coherence is what makes structural realism operational: a structural form that appears only in the system it was designed for is not structurally robust; a form that appears across multiple substrates is. Parsimony rules out theories that derive too little from too many axioms. Comprehensiveness rules out theories that fail to recover known behavior in the limits where established equations apply.

Removing any one criterion produces a recognizable failure mode of theory. Removing internal consistency permits mathematically broken work. Removing reproducibility permits private results. Removing generative scope permits sterile restatement. Removing cross-domain coherence permits substrate-locked metaphor. Removing parsimony permits unconstrained model-fitting. Removing comprehensiveness permits theories that contradict established physics in regimes where established physics is reliable. The six together are the minimum set that admits none of those failure modes; the work is evaluated against this minimum.
