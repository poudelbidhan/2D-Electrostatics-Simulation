

# 2D Electrostatics Simulation using Random Walk Monte Carlo



## Overview

This project simulates a 2D electrostatic problem using a Monte Carlo method, specifically the **Random Walk on Grid** technique. It calculates the electric potential distribution within a defined domain containing a charged conductor and a ground plane. Subsequently, it estimates the total electric charge induced on the ground plane.

The simulation solves Laplace's equation (∇²V = 0) for the electric potential `V` in the charge-free regions, subject to the specified boundary conditions.

## Problem Setup

The simulation models the following physical configuration:

1.  **Domain:** A 2D rectangular region defined by `0 <= x <= X` and `0 <= z <= Z`.
2.  **Conductor:** An inner rectangular conductor defined by `Xmin <= x <= Xmax` and `Zmin <= z <= Zmax`.
3.  **Boundary Conditions:**
    *   **Conductor:** Held at a constant potential of **1 Volt** (Dirichlet boundary).
    *   **Ground Plane:** The bottom edge (`z=0`) is held at **0 Volts** (Dirichlet boundary).
    *   **Other Boundaries:** The top (`z=Z`), left (`x=0`), and right (`x=X`) edges are treated as **reflective boundaries** (Neumann boundary condition, ∂V/∂n = 0), meaning random walkers are reflected back into the domain. This often simulates symmetry or perfect insulation.

## Methodology: Random Walk on Grid

The core of the simulation relies on the property that the solution to Laplace's equation at a point is equal to the average potential on a surface surrounding that point. The Random Walk method leverages this:

1.  **Discretization:** The continuous domain is divided into a grid with spacing `h`.
2.  **Random Walks:** For each grid point `(i, j)` outside the conductor:
    *   Multiple random walks (`N_walks`) are initiated.
    *   Each walk takes random steps (up, down, left, right) on the grid.
    *   Walks reflect off the top, left, and right boundaries.
    *   A walk terminates when it hits either the ground plane (0V) or the conductor (1V).
3.  **Potential Estimation:** The potential `V` at the starting point `(i, j)` is estimated as the **average** of the termination potentials (0 or 1) recorded from all `N_walks` starting at that point.
4.  **Charge Calculation:**
    *   The electric field normal to the ground plane is approximated using the finite difference between the potential at the ground (`j=0`, V=0) and the potential at the grid points just above it (`j=1`). `E_normal ≈ -(V[j=1] - V[j=0]) / h = -V[j=1] / h`.
    *   The total charge on the ground is calculated by integrating the surface charge density (`σ = ε₀ε * E_normal`) along the ground plane, approximated by summing the contributions from each segment and multiplying by the permittivity (`ε₀ε`).



## Configuration

Key parameters can be modified directly in the script:

*   `X`, `Z`: Dimensions of the simulation domain.
*   `Xmin`, `Xmax`, `Zmin`, `Zmax`: Boundaries of the inner conductor.
*   `h`: Discretization step size (smaller `h` increases resolution but also computation time).
*   `N_walks`: Number of random walks per grid point (higher `N_walks` improves accuracy but increases computation time).
*   `eps`: Relative permittivity of the medium.

## Output

The script will print the following information upon completion:

*   **Total number of Walks:** The value of `N_walks` used.
*   **Step size:** The value of `h` used.
*   **Total charge on the ground:** The estimated total charge `Q_ground` induced on the `z=0` boundary (in Coulombs, assuming the 2D simulation represents a system with unit length in the third dimension).
*   **Run time:** The total execution time of the simulation in seconds.

## Limitations and Potential Improvements

*   **Statistical Error:** The result depends on `N_walks`. Higher values reduce statistical noise but increase runtime.
*   **Discretization Error:** The accuracy is limited by the grid spacing `h`.
*   **2D Approximation:** This is a 2D simulation.
*   **Boundary Conditions:** Assumes perfect reflective boundaries on the sides and top.
*   **Potential Improvements:**
    *   Add visualization of the potential map (e.g., using Matplotlib).
    *   Implement adaptive step sizes or walk numbers.
    *   Extend to 3D.
    *   Compare with analytical solutions for simpler geometries or with Finite Difference/Finite Element methods.



