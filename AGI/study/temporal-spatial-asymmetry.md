# Temporal and spatial non-locality act asymmetrically

## The finding

The memory kernel introduced by P2 can be non-local in time (the integral over past times) and optionally also non-local in space (a kernel function $G(\mathbf{x} - \mathbf{x}')$ replacing the delta function). Numerical experiments show that these two forms of non-locality have qualitatively opposite effects on the anti-collapse mechanism:

- **Temporal non-locality regularizes collapse.** The integral memory potential with a delta-function spatial kernel produces the anti-collapse phenomenology documented in .
- **Spatial non-locality destroys the regularization.** Replacing the delta-function spatial kernel with a Gaussian or exponential spatial smoothing causes the anti-collapse to fail; the field collapses to lattice scale as if the memory were absent.

This is a clean asymmetry: the two forms of non-locality act in opposite directions despite arising from the same structural principle.

## The numerical demonstration

The two-dimensional anti-collapse setup is run with four different kernel choices, holding all other parameters fixed at $\Lambda = -8$, $\sigma_0 = 1.2$, $(\nu, \lambda) = (10, 1)$, 4000 steps on a $256 \times 256$ lattice.

| Kernel | Final peak | Final FWHM | Dominant wavelength | Regime |
|---|---|---|---|---|
| Local: $\delta(\mathbf{x} - \mathbf{x}')$ | 0.039 | 18.8 | 2.53 | Extended crystal |
| Exponential, $\xi = 1.0$ | 88.9 | 10.6 | 5.89 | **Collapse** |
| Exponential, $\xi = 3.0$ | 62.7 | 10.7 | 5.89 | **Collapse** |
| Gaussian, $\sigma = 1.0$ | 68.4 | 10.8 | 5.89 | **Collapse** |

The local-kernel run shows the standard crystalline anti-collapse signature (final peak ~10⁻²). All three spatially non-local kernels show collapse signatures (final peak ~60–90, comparable to the unmemoried baseline).

Norm conservation is excellent in all four cases ($\sim 10^{-13}$); the difference between the runs is entirely in the dynamics, not in numerical artefacts.

## The mechanism of asymmetry

The anti-collapse mechanism requires that the repulsive memory potential build up at the precise spatial location where the density is rising, so that the delayed outward force is applied exactly at the focal point of the collapse.

In the spatially local case, this is automatic. The auxiliary fields $y_j(\mathbf{x})$ accumulate at the same spatial point $\mathbf{x}$ where $\rho(\mathbf{x})$ is rising. The memory potential rises sharply at the focal point with the density, lags by $\tau_j = 1/\nu_j$, and overshoots locally at the focal point to release the field.

In the spatially non-local case, the auxiliary fields are driven by a spatially smoothed version of $\rho$:

$$
\partial_t y_j = \nu_j ([G \star \rho] - y_j)
$$

where $G \star \rho$ is the spatial convolution. The smoothing spreads the rising density signal across a region wider than the focal point. The memory potential rises over this wider region, not at the focal point itself. When the memory tries to push outward, the outward push is distributed across the smoothed region rather than concentrated at the focal point. The collapse, which is geometrically focused at a single point, is left undefended by the spatially smeared repulsion.

The collapse wins because the trigger of collapse is geometrically localized and the response of the smoothed memory is not.

## What this says about the third principle

P3, coupling is the default, has internal structure. The principle prescribes that the entity cannot be fully isolated; environmental coupling is mandatory. But it does not, as a structural principle, specify whether the coupling should be temporal, spatial, or both. The numerical asymmetry documented here says that the equation has a definite answer to this question: temporal coupling preserves localization; spatial coupling dissolves it.

This is not what would be expected by symmetry alone. Time and space appear in the equation in approximately parallel ways: the wave operator has time derivatives and Laplacians, the memory kernel has temporal and (optional) spatial parts. Naive symmetry would suggest that the two non-localities should be approximately interchangeable. The numerical experiment refutes this expectation: the two are not interchangeable; they have opposite effects on the anti-collapse mechanism.

The structural fact is that the anti-collapse mechanism is geometrically focused. The focal point of the collapse is a single spatial point. The defense against the focal point must be applied at the focal point. Smearing the defense, by any means, including spatially non-local memory, defeats it.

## Implications for cross-domain mappings

The temporal–spatial asymmetry is one of the more distinctive structural features of the equation. It has implications for how the cross-domain mappings in should be read:

- For Bose–Einstein condensates and nonlinear optical media, the memory coupling is typically modeled as local in space (Stoof 1999); this is consistent with the asymmetry result.
- For neural systems, where temporal memory hierarchies are well-established (hippocampal–cortical consolidation, gamma-on-beta-on-alpha hierarchies) but spatial coupling between non-adjacent neurons typically operates through multiple-step interactions rather than direct continuous kernels, the asymmetry is again consistent with the empirical structural setup.
- For attention mechanisms in machine learning, the analogue of the asymmetry is that temporal attention (across past tokens) regularizes long-range dependencies, while spatial attention (across non-adjacent positions within a single timestep) introduces the rank-collapse problem documented in the transformer literature (Dong, Cordonnier & Loukas 2021). 

The structural finding is therefore not isolated to the equation; it appears, with appropriate substrate-specific translation, in each of the domains where the equation's structure has an analogue.
