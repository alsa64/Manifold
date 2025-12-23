# Watertight Manifold

> **Fork Notice**: This fork includes CMake modernization, Eigen compilation fixes for modern compilers, and multi-format mesh support (OBJ, STL, PLY, OFF).

Source code for the paper:

Huang, Jingwei, Hao Su, and Leonidas Guibas. [**Robust Watertight Manifold Surface Generation Method for ShapeNet Models.**](https://arxiv.org/abs/1802.01698), arXiv preprint arXiv:1802.01698 (2018).

## News!
An advanced version has been released in this new [**repo**](https://github.com/hjwdzh/ManifoldPlus).

## ShapeNet Manifold Dataset
We prepare the manifold data for 13 categories from ShapeNetCore. You can download them by running the following script.
```
wget http://download.cs.stanford.edu/orion/Shapenet_Manifold/categories.txt
wget -i categories.txt
```

## Install and Run

Pre-built binaries for Linux, macOS, and Windows are available in [Releases](../../releases). To create a new release, push a tag starting with `v` (e.g., `v1.0.0`).

For Linux and Mac users, run `sh demo.sh` to build and try the manifold example.

### Install

```sh
git clone --recursive -j8 git://github.com/hjwdzh/Manifold
cd Manifold
mkdir build
cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
make
```

### Manifold Software

We take a triangle mesh and generate a manifold. The resolution is the number of leaf nodes of octree. The face number increases linearly with the resolution.

Supported formats: OBJ, STL, PLY, OFF (automatically detected from file extension)

```sh
./manifold input.[obj|stl|ply|off] output.[obj|stl|ply|off] [resolution (Default 20000)]
```

### Simplify Algorithm

Our manifold software generates uniform manifold. For efficiency purpose, a mesh simplification can be used.

```sh
./simplify -i input.obj -o output.obj [-m] [-f face_num] [-c max_cost] [-r max_ratio]
```

Where:

```sh
  -m            Turn on manifold check, we don't output model if check fails
  -f face_num   Add termination condition when current_face_num <= face_num
  -c max_cost   Add termination condition when quadric error >= max_cost
  -r max_ratio  Add termination condition when current_face_num / origin_face_num <= max_ratio
```

### Example:

```sh
./simplify -i input.obj -o output.obj -m -c 1e-2 -f 10000 -r 0.2
```

## Authors
- [Jingwei Huang](mailto:jingweih@stanford.edu)

&copy; Jingwei Huang, Stanford University

**IMPORTANT**: If you use this software please cite the following in any resulting publication:
```
@article{huang2018robust,
  title={Robust Watertight Manifold Surface Generation Method for ShapeNet Models},
  author={Huang, Jingwei and Su, Hao and Guibas, Leonidas},
  journal={arXiv preprint arXiv:1802.01698},
  year={2018}
}
```
