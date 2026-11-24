# Internal vibrational modes of the crystalline state

## What is observed

The crystalline spatial pattern documented in  is not static. Each spatial point within the crystal oscillates in time. Per-pixel temporal Fourier analysis of a densely sampled trajectory reveals a distribution of dominant frequencies with definite structure:

- Range: 0.1 to 7.6 cycles per unit time
- **Median dominant frequency: 0.6 cycles per unit time**
- **Secondary mode locked at exactly 1.0 cycles per unit time**

The total spectrum exhibits a power-law decay with discrete superimposed peaks. Spatial maps of the dominant frequency per pixel show structure: certain lattice regions vibrate predominantly at $\sim 0.3$ cycles per unit time, others at $\sim 1$+ cycles per unit time. The pattern is therefore both spatially periodic and temporally structured.

## Numerical specification

| Parameter | Value |
|---|---|
| Lattice | $256 \times 256$ |
| Box length | $L = 20$ |
| Time step | $dt = 0.0025$ |
| Integration | 4000 steps (10 units of time) |
| Sampling frequency | every step (dense) |
| Initial state | Gaussian, $\sigma_0 = 1.2$ |
| Nonlinearity | $\Lambda = -8$ |
| Memory | $(\nu, \lambda) = (10, 1)$ |
| Conservative | $\gamma_0 = 0$, $T = 0$ |
| Precision | fp64 |

The conservative regime is essential: with dissipation or noise active, the temporal Fourier analysis would be contaminated by stochastic and dissipative effects. The reported spectrum is the spectrum of the deterministic equation.

## Time-frequency separation: 0.6 and 1.0

The two distinguished frequencies, the median 0.6 and the secondary locked 1.0, are not the result of arbitrary cuts in the spectrum. They are robust features that survive multiple sweep choices.

The 0.6 median is the most common dominant frequency across the spatial points of the crystal. It does not correspond to a single global oscillation of the whole pattern; it is the typical frequency of the per-pixel oscillation, averaged across pixels.

The 1.0 secondary mode is a structurally distinct feature. It appears as a sharp secondary peak in the across-pixel histogram of dominant frequencies, locked precisely at 1.0 cycles per unit time. We do not have an analytical derivation of why exactly 1.0 is selected, but candidate explanations involve a resonance between the slow memory mode at $\nu = 0.5$ (relaxation time $\tau = 2$) and the linear dispersion of the lattice. The locking is sharp enough to be unambiguous and is reproduced across different initial seeds.

## Spatial structure of the vibration

The dominant frequency per pixel is not uniform across the crystalline pattern. A spatial map of the dominant frequency reveals that certain regions of the crystal, typically those between adjacent lattice maxima, oscillate at lower frequencies (around 0.3), while other regions, typically the lattice maxima themselves, oscillate at higher frequencies (above 1.0). The spatial pattern of the dominant frequency thus inherits the spatial periodicity of the crystal, but with a different geometry.

This is structurally analogous to phonon modes in a real crystal: different sublattices vibrate at different frequencies, with the overall pattern of vibration determined by the symmetry of the lattice and the local coupling. The present equation's crystal has an analogous structure, with the auxiliary memory fields playing the role of internal coupling.


## What this result is not

This is not noise. The spectrum is computed in a regime where the equation is fully deterministic ($\gamma_0 = 0$, $T = 0$). The frequencies reported are properties of the equation's dynamics, not of stochastic forcing.

This is not a single oscillation of the global pattern. The pattern as a whole has nontrivial temporal variation, but the report is about the per-pixel dominant frequency, averaged or accumulated across the lattice. The global pattern has additional temporal structure not captured in this summary.

This is not a generic feature of NLS-class equations. The bare cubic NLS does not produce stationary crystalline patterns; it produces collapse or dispersion. The memory potential is what stabilizes the crystalline phase, and the multi-modal time-frequency structure is a property of the memory-augmented dynamics, not the bare nonlinear dispersion.

## Structural significance

Discrete vibrational modes in a self-organized crystalline state are characteristic of systems with hierarchical temporal structure, fast oscillations of individual modes superimposed on slow oscillations of the collective configuration. This is the same hierarchical structure that appears in molecular vibrations (high-frequency intra-molecular modes on top of low-frequency intra-cluster modes), in neural oscillations (gamma on top of beta on top of alpha), and in solid-state phonons (optical on top of acoustic). The present equation produces this hierarchy from purely classical field dynamics with multi-mode memory.

The dimensional identification used in , under which one cycle per unit time maps to approximately 111 Hz, places the median dominant frequency at 66 Hz and the secondary at 110 Hz. These values are within the bands measured at the archaeoacoustic sites discussed there. The structural fact, two principal frequency modes in approximately 0.6:1.0 ratio, is dimension-independent; the absolute frequency values are calibration-dependent. The three-dimensional case has different absolute values (median 0.20 in 3D, mapping to 22 Hz under the same calibration), but the same hierarchical structure.
