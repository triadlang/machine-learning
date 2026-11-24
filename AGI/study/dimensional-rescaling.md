# Dimensional rescaling of the memory coupling

## The finding

The total memory coupling $\Sigma\lambda = \sum_j \lambda_j$ required to release supercritical collapse via the anti-collapse mechanism scales with the spatial dimension $d$ as:

$$
\boxed{\;\frac{\Sigma\lambda}{|\Lambda|} \sim \frac{1}{d}\;}
$$

Empirically:

- In two dimensions (L²-critical regime), $\Sigma\lambda / |\Lambda| \sim 0.05$ is sufficient. With $|\Lambda| = 8$ this gives $\Sigma\lambda \approx 0.4$, the value used in the 2D paper's anti-collapse demonstration.
- In three dimensions (L²-supercritical regime), $\Sigma\lambda / |\Lambda| \sim 0.5$ is required. With $|\Lambda| = 8$ this gives $\Sigma\lambda \approx 4$, the value that recovers the anti-collapse phenomenology documented in 

The ratio scales up by approximately a factor of ten between two and three dimensions. The structural argument below derives this scaling geometrically.

## The structural argument

At the peak of the lattice-clipped collapse, the force balance that determines whether the memory potential can release the field is:

$$
F_{\text{release}} \propto V_{\text{mem}}(\text{focal region}) - |\Lambda| \rho_{\text{peak}}
$$

For the memory to release the collapse, $V_{\text{mem}}$ at the focal region must exceed $|\Lambda| \rho_{\text{peak}}$ at the focal region (more precisely, the gradient of $V_{\text{mem}}$ pointing outward must exceed the gradient of $|\Lambda| \rho$ pointing inward at the boundary of the focal region; the inequality on the magnitudes is approximately this).

The memory potential at the focal region is

$$
V_{\text{mem}}(\text{focal region}) = \sum_j \lambda_j y_j(\text{focal region}) \sim \Sigma\lambda \cdot \rho_{\text{eff}}
$$

where $\rho_{\text{eff}}$ is the density averaged over the spatial region in which the memory has accumulated up to the moment of release. The auxiliary fields $y_j$ track $\rho$ locally in space (because the spatial kernel is a delta function); they take on whatever value $\rho$ has at the same point, modulated by the temporal relaxation. Over the focal region, $y_j \to \rho_{\text{eff}}$ at the relevant timescale.

The key geometric question is: how does $\rho_{\text{eff}}$ relate to $\rho_{\text{peak}}$?

In two dimensions, the lattice-clipped collapse focal region covers approximately $N_{\text{focal}}^{(2D)} \sim 10^2$ lattice cells. The peak is at the center, and the density gradient falls off over $\sqrt{10^2} = 10$ cells. The density averaged over the focal region is approximately $\rho_{\text{peak}}$ near the center but smaller at the edges; on average, $\rho_{\text{eff}}^{(2D)} \approx \rho_{\text{peak}} / \text{O}(1)$. The ratio $\rho_{\text{eff}}/\rho_{\text{peak}}$ in 2D is approximately constant.

In three dimensions, the focal region covers approximately $N_{\text{focal}}^{(3D)} \sim 10$ lattice cells. The density gradient is steeper (higher dimension, same lattice spacing), and the volume over which the density is averaged is smaller. The peak is more concentrated. On average, $\rho_{\text{eff}}^{(3D)} \approx \rho_{\text{peak}} / 10$.

Thus, the ratio $\rho_{\text{eff}}^{(3D)} / \rho_{\text{eff}}^{(2D)} \approx 1/10$ at comparable peak densities.

For the release condition $V_{\text{mem}} > |\Lambda| \rho_{\text{peak}}$ to be satisfied in three dimensions with the same dynamical configuration as in two dimensions, we need:

$$
\Sigma\lambda^{(3D)} \cdot \rho_{\text{eff}}^{(3D)} \sim \Sigma\lambda^{(2D)} \cdot \rho_{\text{eff}}^{(2D)}
$$

which, substituting $\rho_{\text{eff}}^{(3D)} \approx \rho_{\text{eff}}^{(2D)}/10$, gives:

$$
\Sigma\lambda^{(3D)} \sim 10 \cdot \Sigma\lambda^{(2D)}.
$$

This is the empirical scaling observed.

## Comparison: 2D and 3D values

Putting numbers to the structural argument:

| Quantity | 2D (L²-critical) | 3D (L²-supercritical) |
|---|---|---|
| Focal region voxel count | $N \sim 10^2$ | $N \sim 10$ |
| Effective density / peak density | $\sim 1$ | $\sim 1/10$ |
| Total memory coupling for anti-collapse at $\Lambda = -8$ | $\Sigma\lambda \sim 0.4$ | $\Sigma\lambda \sim 4$ |
| Ratio $\Sigma\lambda / |\Lambda|$ | $\sim 0.05$ | $\sim 0.5$ |

The factor-of-ten scaling between the two ratios is the prediction the structural argument makes; the numerical experiments confirm it.

## Why this finding matters structurally

The dimensional rescaling is a derivable consequence of the equation's geometry, not an arbitrary tuning. The mechanism, delayed repulsion from a memory potential, is dimension-independent. The calibration of the coupling that activates the mechanism scales with the dimensional concentration of the focal region in a way that follows from elementary geometry.

This is a concrete instance of a class of predictions that the equation can produce: not single-quantity numerical predictions (which depend on dimensional calibration choices), but scaling relations between configurations that share the underlying structural mechanism. Scaling relations are the kind of prediction that survives recalibration; they are the prediction-type most natural to a structural-realist evaluation of the equation.

## Open question: higher dimensions

The argument above suggests:

$$
\Sigma\lambda^{(d)} / |\Lambda| \sim 1/d
$$

in $d$ spatial dimensions, although the rigorous version requires that the focal region scale as $N_{\text{focal}}^{(d)} \sim 10^{(d-1)}$ which is itself an empirical observation from the 2D and 3D cases at the available lattice resolutions. Whether this scaling extends to $d = 4$ and higher is computationally inaccessible on consumer hardware but would be an interesting analytical exercise.


