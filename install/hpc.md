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

## Running LEM on HPCS (Wilkes GPU)

1. Run the following command to load required module files for LEM.

```bash
module use /scratch/cb-geo/modulefiles
module load gcc/5.3.0
module add intel/mkl/11.3.3.210
module add lem/wilkes
```

2. The LEM package can be called directly by running `lem` 

## Compile and Run LEM on Wilkes

1. Load the required modules:

```bash
module use /scratch/cb-geo/modulefiles
module load gcc/5.3.0
module add intel/mkl/11.3.3.210
module add cuda/8.0
module add vtk/wilkes
module add eigen
module add voro++
```

2. Configure `CMake` and create a make file

```bash
mkdir build && cd build && cmake -DCMAKE_BUILD_TYPE=Release ..
```

3. Run `make clean && make -jN` (where N is the number of cores)

3. Run lem `./lem -d /path/to/inputfile/ -s solver_type`, for e.g., `./lem -d ../benchmarks/fracture_200/ -s CG_MKL`. See [LEM solvers](../solvers/solvers.md) for more details.

4. Run lemtest `ctest -VV -S` or `./lemtest` to run test cases.

## Job submission
### Darwin (CPU) cluster
On Darwin HPCS, use the [CPU Darwin SLURM submission file](https://raw.githubusercontent.com/cb-geo/hpc-scripts/master/lem.txt) or [GPU Wilkes SLURM submission file](https://raw.githubusercontent.com/cb-geo/hpc-scripts/master/lem_gpu.txt)
