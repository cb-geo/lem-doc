# Installation on HPC Darwin
## Prerequisites
### External libraries
* [Voro++](http://math.lbl.gov/voro++/)
* [Eigen](http://eigen.tuxfamily.org/)
* [Intel Threaded Building Blocks (TBB)](https://www.threadingbuildingblocks.org/)

### Compiler and build chain
* CMake 3.1 or higher
* GCC 4.8 or higher (C++11 support)

## Install prerequisites
### Voro++
0. Create a folder called `workspace` in `/scratch` if it doesn't exist (`mkdir -p /scratch/<userid>/workspace`).

1. Download [voro++](http://math.lbl.gov/voro++/download/) to `/scratch/workspace`: `wget http://math.lbl.gov/voro++/download/dir/voro++-0.4.6.tar.gz`.

2. Extract voro++: `tar -xvzf voro++0.4.6.tar.gz`.

3. Open `voro++0.4.6` folder and edit the `config.mk` file:

* Crerate a folder called `voro++` in `workspace`: `mkdir -p /scratch/<userid>/workspace/voro++`

* Edit `config.mk` file (line #20) change `PREFIX=/usr/local` to `PREFIX=/scratch/<userid>/workspace/voro++`. Use `nano` or `vim` to edit the file.

4. While in `voro++0.4.6`, run compile and install commands:

    ```bash
    make
    make install
    ```
5. This will install voro++ in `/scratch/<userid>/workspace/voro++` folder.


### Eigen
1. Create a folder in user `scratch` called `eigen`: `mkdir -p /scratch/<userid>/workspace/eigen/include/eigen3`

2. In workspace `/scratch/<userid>/workspace` download the latest version of Eigen from the mercurial server: `hg clone https://bitbucket.org/eigen/eigen/ eigen3`
 
3. From eigen3 folder copy Eigen to the workspace directory: `cp -R Eigen/ /scratch/<userid>/workspace/eigen/include/eigen3/`


## Load modules on HPCS

This sections describes on getting the latest version of compilers and build chains installed on HPCS Darwin. See [HPCS instructions on modules](http://www.hpc.cam.ac.uk/using-clusters/quick-start#section-3) for further information.

1. Git version 1.8 or higher is required: `module load git/2.8.2`, otherwise you encounter 403 error. Git is required to clone the repository. 

2. CMake version 3.1 or higher: `module load cmake/3.4.3`

3. Load C++11 compatible version of gcc: `module load gcc/5.3.0`

4. Intel TBB for parallelisation support: `module load intel/cce/16.0.3.210`



## Compile and Run

1. Compile `CMake` for custom external library paths

```bash
mkdir build && cd build
cmake -DCMAKE_BUILD_TYPE=Release -DVOROPLUSPLUS_INCLUDE_DIR=/home/<userid>/scratch/workspace/voro++/include/voro++/ -DVOROPLUSPLUS_LIBRARIES=/home/<userid>/scratch/workspace/voro++/lib/libvoro++.so -DEIGEN3_HEADER_PATH=/home/<userid>/scratch/workspace/eigen/include/eigen3/ ..
```

2. Run `make clean && make -jN` (where N is the number of cores)

3. Run lem `export OMP_NUM_THREADS = N; ./lem -d /path/to/inputfile/` (where N is the number of cores)

4. Run lemtest `ctest -VV -S` or `./lemtest` to run test cases.
