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

6. In `/scratch` directory, create a folder called modules: `mkdir -p /scratch/<userid>/modules`. 

7. Add a file `voro++` to the `/scratch/<userid>/modules/` with the following content (make sure to edit the root path:

```tcl
#%Module -*- tcl -*-
module-whatis "voro++"
set root /scratch/<userid>/workspace/voro++
prepend-path PATH $root/bin
prepend-path INCLUDE $root/include
prepend-path LD_LIBRARY_PATH $root/lib
prepend-path MANPATH $root/man
```

8. To make the module available for use run `module use /scratch/<userid>/modules`

9. To load voro++ `module load voro++`

### Eigen
1. Create a folder in user `scratch` called `eigen`: `mkdir -p /scratch/<userid>/workspace/eigen`

2. In workspace `/scratch/<userid>/workspace` download the latest version of Eigen from the mercurial server: `hg clone https://bitbucket.org/eigen/eigen/ eigen3`
 
3. Change working directory to eigen `cd /scratch/<userid>/workspace/eigen/`

4. Run CMake from eigen pointing to the path where `CMakeLists.txt` file is present: `cmake -DCMAKE_INSTALL_PREFIX=/home/<userid>/scratch/workspace/eigen/ ../eigen3/`

5. Once CMake is configured, run `make install`. This will install eigen locally.

6. Add a file `eigen` to the `/scratch/<userid>/modules/` with the following content (make sure to edit the root path:

```tcl
#%Module -*- tcl -*-
module-whatis "eigen"
set root /scratch/<userid>/workspace/eigen
prepend-path INCLUDE $root/include/eigen3
```
7. To make the module available for use run `module use /scratch/<userid>/modules`

8. To load eigen `module load eigen`


## Load modules on HPCS

This sections describes on getting the latest version of compilers and build chains installed on HPCS Darwin. See [HPCS instructions on modules](http://www.hpc.cam.ac.uk/using-clusters/quick-start#section-3) for further information.

1. Git version 1.8 or higher is required: `module load git/2.8.2`, otherwise you encounter 403 error. Git is required to clone the repository. 

2. CMake version 3.1 or higher: `module load cmake/3.4.3`

3. Load C++11 compatible version of gcc: `module load gcc/5.3.0`

4. Intel TBB for parallelisation support: `module load intel/cce/16.0.3.210`

> **Note** All module load commands maybe included in `~/.bashrc` file so they are loaded at the start of each session.

## Compile and Run

1. Compile `CMake` for custom external library paths

```bash
mkdir build && cd build && cmake -DCMAKE_BUILD_TYPE=Release ..
```

2. Run `make clean && make -jN` (where N is the number of cores)

3. Run lem `export OMP_NUM_THREADS = N; ./lem -d /path/to/inputfile/` (where N is the number of cores)

4. Run lemtest `ctest -VV -S` or `./lemtest` to run test cases.
