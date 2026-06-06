# triad-ml

Machine learning stack built on the Triad equation. Part of the [TriadLang](https://github.com/triadlang/triadlang) project.

```
i·ℏ·∂_t Ψ = [-ℏ²/(2m)∇² + V_ext + Λ|Ψ|² + V_mem + α(-Δ)^(σ/2) - iΓ]Ψ + η
```

This is the ML layer alone, separated out from the main TriadLang repo so you can use the tensor library, neural network primitives, and the `TriadSSM` (the equation as a sequence layer) without pulling in the language frontend, compiler, or CLI.

---

## What is here

A complete deep learning stack with reverse-mode autograd, written in pure NumPy. No PyTorch dependency. CuPy is optional for GPU.

```
src/triad_ml/
├── tensor.py       TriadTensor with reverse-mode autograd
├── nn.py           Linear, Conv1d, Conv2d, Sequential, LayerNorm,
│                   Embedding, MultiHeadAttention, Transformer,
│                   TriadSSM, TriadSSMBlock, SGD, Adam
├── losses.py       MSE, cross-entropy, BCE, KL divergence
├── metrics.py      Accuracy, precision/recall, perplexity
├── data.py         TextDataset, batching utilities
├── trainer.py      Training loop with checkpointing
├── generate.py     Autoregressive generation utilities
├── language.py     CharTokenizer, TriadLM language model
├── serialization.py  Weight save/load
├── ml_device.py    NumPy / CuPy backend proxy (xp)
├── backend.py      Device selection
├── surrogate.py    PDE surrogate models (TriadFullBlock,
│                   MNOSurrogate, FidelityGate)
├── kinetic_surrogate.py  Spectral kinetic surrogate
├── forward.py      Inference utilities
├── gpu_fused.py    Fused CUDA kernels for the solver path
├── gpu_kernel.py   Lower-level GPU kernel helpers
├── model_loader.py  Weight loading from external formats
└── core/           The Triad equation solver
    └── solver.py   Split-step Strang integrator (1D/2D/3D)
└── physics/        Field measurements
    └── observables.py  Crystallinity, k*, IPR, energy, FWHM
```

---

## The piece worth pointing at: `TriadSSM`

This is the part that is specific to this project. The recurrence is the Triad equation discretised in time, with the physical coefficients exposed as learnable parameters:

```python
from triad_ml.nn import TriadSSM
from triad_ml.tensor import randn

ssm = TriadSSM(d_model=64, d_state=32, n_memory=3, coupling=True)
x = randn(batch=2, seq_len=128, dim=64)
y = ssm(x)
```

What the layer does internally, per timestep:

```
re, im  = cmul(Ah, re, im)              # P1: complex oscillator (kinetic)
rho     = re² + im²
y_j     = d_j · y_j + (1 - d_j) · rho   # P2: OU memory fields
V       = Lambda · rho + Σ_j lam_j · y_j # P2: cubic + memory potential
V       = V + kappa · couple(rho)       # P3: inter-channel coupling
re, im  = re · cos(V) + im · sin(V),    # phase rotation by V
          im · cos(V) - re · sin(V)
y_j     = d_j · y_j + (1 - d_j) · rho_new  # P2: second OU half-step
```

The learnable parameters are the physical constants of the equation:

| Parameter | Symbol | Role |
|---|---|---|
| `omega` | ω | oscillation frequencies (P1) |
| `log_decay` | mag | per-mode amplitude decay (P1) |
| `nu` | ν_j | memory relaxation rates (P2) |
| `lam` | λ_j | memory coupling strengths (P2) |
| `Lambda` | Λ | cubic self-interaction (P2) |
| `kappa` | κ | inter-channel coupling (P3) |

The three pillars are present in every forward pass. They are not switchable.

---

## Standard stack (also included)

The same tensor library handles the usual deep learning primitives. Nothing exotic about these — they exist so a model can be built end-to-end without leaving the package.

```python
from triad_ml.nn import Sequential, Linear, ReLU, Adam
from triad_ml.tensor import tensor, cross_entropy

model = Sequential(Linear(2, 16), ReLU(), Linear(16, 2))
opt = Adam(model.parameters(), lr=0.05)

for epoch in range(150):
    logits = model(x)
    loss = cross_entropy(logits, y)
    opt.zero_grad()
    loss.backward()
    opt.step()
```

---

## Install

```bash
pip install -e .
```

For GPU:

```bash
pip install -e ".[gpu]"
```

For tests:

```bash
pip install -e ".[dev]"
pytest tests/
```

182 tests pass on a fresh install. They cover the tensor autograd, the neural network primitives, the trainer loop, and the broader infrastructure.

---

## Examples

In `examples/`:

| File | What it does |
|---|---|
| `xor_train.tri` | XOR via Sequential MLP + Adam |
| `nn_test.tri` | Linear, Conv1d, Sequential, full XOR training |
| `tensor_test.tri` | Autograd primitives |
| `transformer_baseline.tri` | Transformer block |
| `train_full.tri` | TriadLM character-level language model (6 TriadSSMBlocks) |
| `xor_native.tri` | Native (compiled C) XOR training |
| `benchmark.tri` | Throughput measurement on real ops |

The `.tri` files use TriadLang syntax. To run them you still need the main TriadLang package. The Python API in `triad_ml` works directly without that.

---

## Native C path

The C sources in `native/c/` reimplement the autograd, neural network layers, and training loop with the same numerics as the Python side, intended to be compiled with the main TriadLang native runtime. The headers `triad_ml.h` and `triad_train.h` declare the C ABI.

| File | Lines | What it is |
|---|---|---|
| `src/triad_autograd.c` | 2183 | Tensor + reverse-mode AD with topological sort |
| `src/triad_nn.c` | 781 | Linear, Sequential, activations, optimizers |
| `src/triad_train.c` | 194 | Training loop helpers |

Building requires the rest of the TriadLang native runtime (`triad_rt.h`, Boehm GC, FFTW3). The Python package works standalone.

---

## What this package does not include

- The TriadLang language frontend (`lexer`, `parser`)
- The compiler (`lower`, `c_codegen`, `triadc`)
- The CLI (`triad run`, `triad compile`, `triad repl`)
- The multi-substrate runtime (`MultiRuntime`, `CouplingEdge`, `Segment`)
- The cross-domain regimes (`regime_HodgkinHuxley`, `regime_LSV_market`, etc.)
- The SAT solver, the calibrator, the steering controller

For those, use the main [TriadLang](https://github.com/triadlang/triadlang) repository. This package is the ML primitives plus the underlying equation solver they sit on, nothing more.

---

## License

Same terms as the parent TriadLang project. See `LICENSE`.
