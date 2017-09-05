# Preprocessing

Lattice Element Method (LEM) code use JSON file type for configuring the input. The flag `-i` is used to specify the input `JSON` file.


## Usage

```
   ./lem  -d <Dimension> -f <Working_folder> [-i <input_file>] -s <solver_type>  [--] [--version] [-h]
```

Where:

```
   -d <Dimension>,  --dimension <Dimension>
          (required)  Problem dimension

   -f <Working_folder>,  --working_dir <Working_folder>
     (required)  Current working folder

   -i <input_file>,  --input_file <input_file>
       Input JSON file [lem.json]

   -s <solver_type>,  --solver <solver_type>
         (required)  solver type

   --,  --ignore_rest
     Ignores the rest of the labeled arguments following this flag.

   --version
     Displays version information and exits.

   -h,  --help
     Displays usage information and exits.
```

The LEM configuration defines id, input files, bounding box, element, node sets and boundary conditions:

```json
{
  "title": "Uniaxial stress controlled tension test 50x50x50 normal distribution",
  "mesh": {
    "id": 0,
    "input_files": {
      "nodes": "input/nodes.txt",
      "elements": "input/elements.txt"
    },
    "bounding_box" : [0.0, 50.0, 0.0, 50.0, 0.0, 50.0],
    "element" : {
      "type" : "Beam",
      "alpha" : 1.0,
      "beta"  : 1.0,
      "gamma" : 1.0,
      "tensile_strength" : 2000,
      "cohesion" : 2000,
      "friction_angle" : 0.0,
      "Emicro" : 2.0E+7,
      "distribution" : {
        "type" : "uniform",
        "sigma" : 0.05,
        "mu" : 1.0,
        "min_threshold" : 0.2
      }
    },
    "reconnection" : {
      "status" : true,
      "threshold": 1.0E-05
    },
    "boundary_conditions" : [
      {
        "type" : "restrain",
        "node_set" : "-z",
        "restrain" : [false, false, true, false, false, false]
      },
      {
        "type" : "pressure",
        "node_set" : "+z",
        "pressure" : 1700,
        "dir" : 2,
        "face" : 5
      }
    ]
  },
  "analysis" : {
    "type" : "pressure",
    "loading" : {
      "node_set" : "+z",
      "pressure" : 1700,
      "dir" : 2,
      "face" : 5,
      "strain_node_set" : ["+z", "-z"],
      "max_steps" : 5,
      "nreassemble_stiffness": 1,
      "max_threshold_lattices" : 20,
      "max_breakable_lattices" : 20
    }
  },
  "solver" : {
    "max_iterations" : 10000,
    "tolerance" : 2.5E-03
  },
  "post_processing" : {
    "output_steps" : 100,
    "results_path" : "results/"
  }
}
```

## Title \[optional\]

```json
  "title": "Uniaxial stress controlled tension test 50x50x50 normal distribution",
```

Descriptive name of the analysis. This is an optional argument, but is recommended to provide a meaningful title to the analysis.

## Mesh

The `mesh` object specifies the global mesh parameters.

### ID
The object `id` denotes the `mesh` id, in case multiple meshes are used.

### Input Files

The input files is an argument in the `mesh` element,  which contains the directory of the input files with the nodes and elements information.

```json
    "input_files": {
      "nodes": "input/nodes.txt",
      "elements": "input/elements.txt"
    }
```

#### Nodes

The nodes is an argument in the `mesh` element,  which contains the directory of the input file  with the nodes coordinates.

```json
      "nodes": "input/nodes.txt"
```

#### Elements \[optional\]

The elements is an optional argument in the `mesh` element,  which contains the directory of the input file  with the nodal incidence of the elements.This is an optional argument, if not specified, the LEM code will create  the input file  with tnodal incidence of the lattice elements.

```json
      "elements": "input/elements.txt"
```

### Bounding box \[optional\]

The bounding box is an optional argument in the `mesh` element, which defines the minimum and maximum values of the box in three principal axes which encloses the LEM nodes. This is an optional argument, if not specified, the LEM code will compute the bounding box.

```json
    "bounding_box" : [0.0, 50.0, 0.0, 50.0, 0.0, 50.0]
```

### Element

The element is an argument in the `mesh` element, which defines the material properties information.

```json
    "element" : {
      "type" : "Beam",
      "alpha" : 1.0,
      "beta"  : 1.0,
      "gamma" : 1.0,
      "tensile_strength" : 2000,
      "cohesion" : 2000,
      "friction_angle" : 0.0,
      "Emicro" : 2.0E+7,
      "distribution" : {
        "type" : "uniform",
        "sigma" : 0.05,
        "mu" : 1.0,
        "min_threshold" : 0.2
      }
    }
```

#### Type

The type is an argument in the `mesh` element, which defines the lattice element type \(Beam or Spring\).

```json
      "type" : "Beam"
```

#### Alpha

The parameter `alpha` is the ratio of stiffness between axial spring and shear spring.

```json
      "alpha" : 1.0
```

#### Beta

The parameter `beta` is a scalar introduced to modify the contribution of rotation stiffness.

```json
      "beta"  : 1.0
```

#### Gamma

The  coefficient `gamma` is used to  reduce the normal and shear stiffness in tensile failure and shear failure. More information is can be found in [https://lem-doc.cb-geo.com/stiffness/stiffness.html](https://lem-doc.cb-geo.com/stiffness/stiffness.html)

```json
      "gamma" : 1.0
```

#### Tensile strength

The tensile strength is the maximum stress that the lattice element can withstand while being stretched or pulled before breaking.

```json
      "tensile_strength" : 2000
```

#### Cohesion

Is the material cohesion that is used in the conventional Mohr-Columnb failure citeria.

```json
      "cohesion" : 2000
```

#### Friction angle

Is the friction angle of the material that is used in the conventional Mohr-Columnb failure citeria.

```json
      "friction_angle" : 0.0
```

#### Emicro

The `Emicro` is the Micropolar elasticity module of the lattice element.

```json
      "Emicro" : 2.0E+7
```

#### Distribution

LEM code includes the option to choose from three probability distribution functions \(normal, lognormal, and uniform\) to represent the heterogeneity in stiffness, strength, or both. The parameters used in the distribution function are `sigma`, `mu` and `min\_threshold`, where `sigma` is the standard deviation, `mu` is the mean or expectation of the distribution and `min_threshold` is the minimum allowable probability value.

```json
      "distribution" : {
        "type" : "normal",
        "sigma" : 0.05,
        "mu" : 1.0,
        "min_threshold" : 0.2
      }
```

### Reconnection

The re-connection is an argument in the `mesh` element, `status: yes` adds an increase in stiffness from `0` stiffness, when  an element gets reconnected.

```json
    "reconnection" : {
      "status" : true,
      "threshold": 1.0E-05
    }
```

### Boundary conditions

Boundary conditions are used to prescribe values of basic solution variables: displacements and rotations for `restrain` boundary or `pressure` to apply initial load.

```json
    "boundary_conditions" : [
      {
        "type" : "restrain",
        "node_set" : "-z",
        "restrain" : [false, false, true, false, false, false]
      },
      {
        "type" : "pressure",
        "node_set" : "+z",
        "pressure" : 1700,
        "dir" : 2,
        "face" : 5
      }
    ]
```

## Boundary node sets

Cartesian boundary node sets are automatically created based on the list of nodes. List of automatically created boundary node sets are: `-x`, `+x`, `-y`, `+y`, `-z` and `+z`.

## Analysis

The analysis argument define what load type is applied as `displacement` or `pressure`, maximum number of steps, maximum number of breakable lattices by step and others options specified by the user.

```json
  "analysis" : {
    "type" : "pressure",
    "loading" : {
      "node_set" : "+z",
      "pressure" : 1700,
      "dir" : 2,
      "face" : 5,
      "strain_node_set" : ["+z", "-z"],
      "max_steps" : 5,
      "nreassemble_stiffness": 1,
      "max_threshold_lattices" : 20,
      "max_breakable_lattices" : 20
    }
  }
```

# Sample input files

* [https://github.com/cb-geo/lem-benchmarks](https://github.com/cb-geo/lem-benchmarks)
