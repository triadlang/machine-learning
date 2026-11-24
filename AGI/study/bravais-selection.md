# Bravais lattice selection in three dimensions

## What is observed

The released crystalline configuration in three dimensions, at $\Lambda = -8$ across the swept memory coupling range $\Sigma\lambda \in [0.5, 1.5]$, consistently selects **body-centered cubic (BCC)** symmetry. The BCC score is approximately 0.44, with a gap of approximately +0.13 over the next-best Bravais option (face-centered cubic, score ~0.30) and a larger gap over hexagonal close-packed (~0.30) and simple cubic (~0.22).

## The detection algorithm

The Bravais lattice identification is performed by the function `detect_bravais_3d` in . The algorithm:

1. Computes the three-dimensional power spectrum $|\Psi(\mathbf{k})|^2$ of the final-state field.
2. Determines the dominant non-zero radial wavenumber $k^*$ from the spherically-averaged spectrum, with a low-$k$ cutoff to ignore the trivial zero mode.
3. Samples the power on a thin spherical shell at $k^*$ (shell width approximately $\pm 10\%$ of $k^*$).
4. For each candidate Bravais lattice (SC, BCC, FCC, HCP), computes a score equal to the fraction of shell power aligned with that lattice's canonical $k$-space peak directions, with a smooth angular window (cosine threshold $\sim 0.15$).
5. Reports the best-matching lattice plus the full score dictionary.

The canonical $k$-space signatures for the four Bravais options are:

- **SC**: 6 peaks at $\pm \hat{x}, \pm \hat{y}, \pm \hat{z}$ (vertices of an octahedron).
- **BCC**: 12 peaks at the (1,1,0)-type permutations (centered on edges of a cube).
- **FCC**: 8 peaks at the (1,1,1)-type permutations (vertices of a cube).
- **HCP**: 6 in-plane hexagonal peaks plus 2 axial peaks.

The score is a fraction in $[0, 1]$. A perfectly clean BCC crystal would give BCC ≈ 1.0 and all others ≈ 0.1–0.3 (residual signal from imperfect angular alignment). A fully disordered state would give all four scores in the 0.2–0.4 range with no clear winner.

## The numerical sweep

At $\Lambda = -8$, $\sigma_0 = 0.5$, on a $128^3$ lattice with $L = 20$, $dt = 0.0025$, 6000 integration steps, fp32 precision, the swept memory coupling produces:

| $\Sigma\lambda$ | Peak | FWHM | Best Bravais | Score | Gap to next |
|---|---|---|---|---|---|
| 0.5 | 66.5 | 0.94 | BCC | 0.438 | +0.13 (vs FCC 0.305) |
| 1.0 | 51.9 | 0.94 | BCC | 0.441 | +0.14 (vs FCC 0.303) |
| **1.5** | **0.0028** | **0.94** | **BCC** | **0.439** | **+0.13 (vs FCC 0.305)** |
| 2.0 | 0.0010 | 0.94 | BCC | 0.336 | +0.01 (vs FCC 0.330) |
| 2.5 | 0.0018 | 1.56 | BCC | 0.336 | +0.01 (vs FCC 0.329) |
| 3.0 | 0.0010 | 1.56 | BCC | 0.335 | +0.01 (vs FCC 0.328) |

Three regimes are visible:

- **$\Sigma\lambda \le 1.0$**: collapsed (lattice-trapped) regime. The high BCC score reflects the angular alignment of the collapse-clipped spike's angular signature with BCC directions.
- **$\Sigma\lambda = 1.5$**: the released crystalline window. The peak has dropped four orders of magnitude (from $\sim 50$ to $\sim 10^{-3}$) while the BCC score is preserved at 0.44, with the +0.13 gap intact. This is the structurally significant regime: the field has released from the collapse and stabilized as a BCC-symmetric extended pattern.
- **$\Sigma\lambda \ge 2.0$**: dispersed regime. The field has spread out so much that the angular distribution of $k$-space power has become approximately isotropic. The BCC score drops to 0.336, with the gap to FCC closing to less than 0.01. This is the disorder regime in the language of the detection algorithm.

## The crystalline window

The narrowness of the crystalline window, only $\Sigma\lambda \approx 1.5$ produces both peak release and BCC preservation, is consistent with the dimensional rescaling argument in . The three-dimensional supercritical collapse is aggressive, and the parameter range over which the memory regularization holds the field in a stable, extended, ordered configuration is narrower than the corresponding range in two dimensions.

## The dominant wavenumber across regimes

The dominant wavenumber $k^*$ steps through three distinct values as $\Sigma\lambda$ increases:

- $\Sigma\lambda \le 0.5$: $k^* = 2.449$, wavelength $\lambda_{\text{wave}} = 2.566$ (collapse-pulse FFT signature)
- $\Sigma\lambda \in [0.7, 1.0]$: $k^* = 1.904$, wavelength $\lambda_{\text{wave}} = 3.299$ (still collapsed, but starting to broaden)
- $\Sigma\lambda = 1.5$: $k^* = 1.360$, wavelength $\lambda_{\text{wave}} = 4.619$ (crystalline window)
- $\Sigma\lambda \ge 2.0$: $k^* = 0.816$, wavelength $\lambda_{\text{wave}} = 7.698$ (dispersed regime; this $k^*$ corresponds roughly to the natural bandwidth of the initial Gaussian)

The crystalline-window wavelength of approximately 4.6 in box units is the three-dimensional analog of the two-dimensional wavelength of approximately 2.95.

## Why BCC and not another Bravais option

A first-principles derivation of why the equation selects BCC rather than FCC, HCP, or SC has not been performed. The selection is observed numerically and is robust across the swept range, but the structural reason, presumably related to the geometry of the most unstable modes of the modulational instability about the unperturbed uniform state in three dimensions, plus the focal geometry of the collapse-release transient, has not been worked out analytically.

This is noted as an open question in , Section 10. A natural next step is to perform a linearized stability analysis of the equation about the uniform state, in three dimensions, with the memory potential active; the most unstable wavevectors of the linearization should match the BCC signature seen in the nonlinear dynamics.

## Structural significance

The spontaneous selection of a specific Bravais lattice, from a continuous initial state with no preferred direction, is a concrete structural prediction of the equation. The equation, with no input from the modeler about which lattice should emerge, produces BCC reproducibly. Had it produced FCC or HCP under the same conditions, the structural content of the prediction would be different. This is the kind of specific structural finding that distinguishes the equation from a generic "produces some crystal" claim.

The selection of BCC, rather than the closer-packing FCC or HCP, may itself be informative. BCC has lower packing density than FCC or HCP, but higher coordination flexibility at the second-nearest-neighbor level. Whether this is the structural reason for the selection or coincidence with the focal-geometry constraints is, as noted, an open question.
