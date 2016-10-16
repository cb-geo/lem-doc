# Installation on HPC Darwin
## Prerequisites
* [Voro++](http://math.lbl.gov/voro++/)
* [Eigen]()
* [Intel Threaded Building Blocks (TBB)]()

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
