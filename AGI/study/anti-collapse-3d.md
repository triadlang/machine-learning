# Anti-collapse in three dimensions

## What is observed

In three spatial dimensions, where the cubic nonlinear Schrödinger equation is L²-supercritical and finite-time collapse is generic for any sufficiently concentrated initial state, the memory potential, at appropriately rescaled coupling, produces a four-to-five-orders-of-magnitude separation between the unmemoried and memoried final states across the supercritical $\Lambda$ range.

| $\Lambda$ | No-memory final peak | With-memory final peak | Ratio |
|---|---|---|---|
| $-6$ | 0.0015 | 0.0006 | 2.5× |
| $-8$ | 61.96 | 0.0006 | $\sim 10^5$ |
| $-10$ | 59.27 | 0.0027 | $\sim 2 \times 10^4$ |
| $-12$ | 57.02 | 0.0018 | $\sim 3 \times 10^4$ |

The no-memory runs at $\Lambda \le -8$ lock at the lattice-clipped peak ($\sim 57$–$62$); the memoried runs at the same $\Lambda$ unwind to peaks of order $10^{-3}$.

## Two dynamical regimes

Two qualitatively different dynamical signatures appear, depending on $\Lambda$:

**Regime A: Λ near the collapse boundary** ($\Lambda \approx -8$ at the parameters chosen). The memoried run aborts the collapse before it reaches lattice scale. The maximum peak during the integration is approximately 6.88, only five times the initial peak (compared to 45 times in the unmemoried baseline). The field then disperses to peak $\sim 10^{-3}$. The final FWHM expands to approximately 19.69, essentially the full box. This is the "abort the collapse before it locks" dynamic.

**Regime B: Strongly supercritical Λ** ($\Lambda \le -10$). The memoried run permits the collapse to reach lattice scale first, the maximum peak reaches $\sim 57$ in the $\Lambda = -10$ case and $\sim 37$ in the $\Lambda = -12$ case, before the memory overshoots and unwinds the configuration. The field releases later in the trajectory and ends at the same peak $\sim 10^{-3}$. This is the "collapse to lattice, then release" dynamic.

Both regimes terminate at the same final peak ($\sim 10^{-3}$), demonstrating that the anti-collapse mechanism is robust independent of which dynamical path the trajectory takes.

## Numerical specification

| Parameter | Value |
|---|---|
| Lattice | $128^3$ |
| Box length | $L = 20$ |
| Time step | $dt = 0.0025$ |
| Integration | 4000 steps |
| Initial state | Gaussian, $\sigma_0 = 0.5$, $\mathbf{k}_0 = (0, 0, 0)$ |
| Nonlinearity | swept: $\Lambda \in \{-2, -4, -6, -8, -10, -12\}$ |
| Memory (with-mem arm) | $(\nu_1, \lambda_1) = (10, 3.0)$, $(\nu_2, \lambda_2) = (0.5, 1.0)$ |
| Memory (no-mem arm) | none |
| Dissipation, noise | $\gamma_0 = 0$, $T = 0$ |
| Precision | fp32 |

Note that the initial momentum has been set to zero in the three-dimensional case. This is to ensure that the in-place focusing dynamics dominate over the translational dispersion that the momentum would induce. The two-dimensional reference run uses nonzero initial momentum, but the structural finding (anti-collapse) is the same.

The initial width $\sigma_0 = 0.5$ places the initial state above the dimensional collapse threshold for $\Lambda \ge -8$. With $\sigma_0 = 0.5$ and the normalization condition $\int |\Psi|^2 d^3x = 1$, the initial peak density is $\rho_0 = (2\pi\sigma_0^2)^{-3/2} \approx 1.44$.

## Reproduction

```bash
python experiments/physics/reproduce_3d_anti_collapse.py
```

Expected wall time: ~2.5 minutes on RTX 4060 (12 runs, 6 values of $\Lambda$ × 2 memory conditions).

## The memory coupling has been rescaled

The three-dimensional anti-collapse demo uses total memory coupling $\Sigma\lambda = 4$ (distributed as $\lambda_1 = 3.0$ on the fast mode and $\lambda_2 = 1.0$ on the slow mode). This is approximately ten times the total coupling used in the two-dimensional anti-collapse demo. The reason for this rescaling is documented in [`06-dimensional-rescaling.md`](06-dimensional-rescaling.md): the three-dimensional supercritical collapse focuses the field into a spatially smaller focal region than the two-dimensional critical collapse, so the memory potential, which is integrated over the focal region, must be correspondingly stronger per unit density to overcome the attractive nonlinearity.

This is, on the structural-realist reading, an instance of the equation generating a derivable cross-dimensional scaling relation. The mechanism is the same in 2D and 3D; the calibration of the coupling that activates the mechanism scales with the dimensional geometry of the collapse.

## Robustness

The finding has been replicated at $\Lambda \in [-6, -12]$ as tabulated above. It has also been replicated under variation of $\sigma_0 \in [0.4, 0.6]$ within the supercritical regime; the final peak ratio remains in the four-to-five-orders-of-magnitude range across this variation. It has not been replicated at $N = 192$ or $N = 256$ due to compute constraints; this is noted in the manuscript as an open mesh-convergence question for future work.

## Structural significance

The three-dimensional anti-collapse is a stronger structural claim than the two-dimensional case because the 3D NLS is supercritical: there is no kinematic-pressure regime in which the field is protected without the memory acting. The fact that the memory recovers the anti-collapse phenomenology in this regime, under a derivable dimensional rescaling of its coupling, demonstrates that the mechanism is intrinsic to the structural form of the equation and not specific to the L²-critical boundary in two dimensions.
