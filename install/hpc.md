# Installation on HPC Darwin
## Prerequisites
* [Boost](http://www.boost.org/)
* [Voro++](http://math.lbl.gov/voro++/)
* [Eigen](http://eigen.tuxfamily.org/)
* [Intel Threaded Building Blocks (TBB)](https://www.threadingbuildingblocks.org/)
* [VTK](http://www.vtk.org/)

* [Intel Math Kernel Library](https://software.intel.com/en-us/intel-mkl/) optional
* [CUDA](https://www.nvidia.com/object/cuda_home_new.html) optional

## Compiler and build chain
* CMake 3.1 or higher
* GCC 4.8 or higher (C++11 support)

## Load modules on HPCS using Spack

This sections describes on getting the latest version of compilers and build chains installed on HPCS Darwin. See [HPCS instructions on modules](http://www.hpc.cam.ac.uk/using-clusters/quick-start#section-3) for further information.

1. [Spack] has been installed on `/scratch/cb-geo` folder. Configure `spack`: `. /scratch/cb-geo/spack/share/spack/setup-env.sh`

2. Load `lem` and preconfigured dependencies using `spack`: `spack load lem/develop`.

or 

2. Load `lem` using `module av lem` and `module load lem/develop`

## Compile and Run

1. Compile `CMake` for custom external library paths

```bash
mkdir build && cd build && cmake -DCMAKE_BUILD_TYPE=Release ..
```

2. Run `make clean && make -jN` (where N is the number of cores)

3. Run lem `./lem -d /path/to/inputfile/ -s solver_type`, for e.g., `./lem -d ../benchmarks/fracture_200/ -s CG_MKL`. See [LEM solvers](../solvers/solvers.md) for more details.

4. Run lemtest `ctest -VV -S` or `./lemtest` to run test cases.

## Job submission
### Darwin (CPU) cluster
On Darwin HPCS, use the [CPU Darwin SLURM submission file](https://raw.githubusercontent.com/cb-geo/hpc-scripts/master/lem.txt) or [GPU Wilkes SLURM submission file](https://raw.githubusercontent.com/cb-geo/hpc-scripts/master/lem_gpu.txt)
