# 1D Quantum Wavepacket Simulation (Crank–Nicolson)

This project simulates the time evolution of a one-dimensional quantum
wavepacket governed by the time-dependent Schrödinger equation.
The equation is solved numerically using the **Crank–Nicolson method**,
an implicit finite-difference scheme that is stable and second-order
accurate in time.

The simulation demonstrates fundamental quantum phenomena such as:
- Free-particle propagation
- Reflection and transmission at potential steps and barriers
- Quantum tunnelling
- Wavepacket dispersion
- Probability conservation on a closed domain

Animations of the wavefunction and probability density are generated
using Matplotlib.

---

## 📐 Physical Model

The system is described by the one-dimensional time-dependent
Schrödinger equation,

iħ ∂ψ(x,t)/∂t = [ −(ħ² / 2m) ∂²/∂x² + V(x) ] ψ(x,t).

The initial state ψ(x,0) is chosen to be a Gaussian wavepacket with a
specified initial position, width, and mean momentum. The external
potential V(x) may represent free space, a step potential, a finite
potential barrier, or a finite square well.

---

## 🧮 Numerical Method (Crank–Nicolson)

The spatial domain is discretised on a uniform grid,

x_j = x_min + j Δx.

The second spatial derivative is approximated using a second-order
central finite difference, leading to a tridiagonal (sparse)
Hamiltonian matrix of the form

H = −(ħ² / 2m) D_xx + diag(V),

where D_xx is the finite-difference Laplacian operator.

Time evolution is performed using the Crank–Nicolson scheme,

(I + i Δt H / 2ħ) ψ^(n+1) = (I − i Δt H / 2ħ) ψ^n.

At each timestep, a sparse linear system is solved for ψ^(n+1).
For static potentials, the Hamiltonian is time-independent, so the
left-hand matrix can be factorised once and reused to improve
computational efficiency.

---

## ⚖️ Probability and Boundary Effects

For a Hermitian Hamiltonian and in the absence of absorbers, the
Crank–Nicolson method preserves the discrete probability

Σ |ψ_j|² Δx

up to numerical roundoff errors.

Because the simulation is performed on a finite spatial domain,
unphysical reflections can occur at the boundaries. These can be
mitigated by enlarging the domain so the wavepacket never reaches the
edges, or by applying optional absorbing boundary layers that remove
outgoing probability. This code uses an enlarged domain so the wavepacked never reaches the edges in the timespan of the animation.

---

## ✨ Features

- Gaussian wavepacket initial conditions
- Free, step, barrier, and well potentials
- Sparse-matrix Hamiltonian construction
- Crank–Nicolson time evolution
- Optional absorbing boundaries
- Real-time animation of:
  - Re(ψ)
  - Im(ψ)
  - Probability density |ψ|²
- Animation export as GIF or MP4

---

## 🛠 Requirements

Python 3.9 or newer.

Required Python packages:
- numpy
- scipy
- matplotlib

To save MP4 animations, `ffmpeg` is required.

---

## ▶️ Running the Simulation

1. Set the physical and numerical parameters near the top of the script:
   - spatial domain
   - grid spacing Δx
   - timestep Δt
   - number of timesteps
   - choice of potential

2. Run the script or notebook.

---

## 📽️ Animation Output

### ▶️ Display animation inline (Jupyter)

To display the animation directly in a Jupyter notebook:

```python
from matplotlib import rc
rc("animation", html="html5")

anim
```

To save the animation as a GIF:

```python
anim.save("filename.gif", writer="pillow", fps=20)
```

To save the animation as a mp4:

```pyhton
anim.save("wavepacket.mp4",writer="ffmpeg",fps=30)
```
To view MP4 inside Jupyter:
```pyhton
from IPython.display import Video
Video("wavepacket.mp4", embed=True)

```

