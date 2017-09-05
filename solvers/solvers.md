# LEM Solvers
Lattice Element Method uses `Conjugate Gradient` approach to solve the system of equations. The matrices and vectors are stored using `Eigen` library. The LEM code has the option to choose from a few different solvers. The solver type has to be passed as a command line argument.

```
lem -d 3 -f /path/to/lem.json -s solver_type
```
The following solver types are available:

* Eigen (Default) `CG_Eigen`
* Intel MKL `CG_MKL`
* CUDA `CG_GPU`

For example, to use the Intel MKL solver: `lem -d 3 -f /path/to/lem.json -s CG_MKL`.
