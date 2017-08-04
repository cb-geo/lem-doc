# Preprocessing

Lattice Element Method (LEM) code use JSON file type for configuring the input. The flag `-i` is used to specify the input `JSON` file. 

```shell
   -i <input_file>,  --input_file <input_file>
     Input JSON file [lem.json]

   -d <Working_directory>,  --working_dir <Working_directory>
     (required)  Current working directory
```

The mesh configuration defines id, input files, bounding box, element, node sets and boundary conditions:

```json
{
  "mesh": {
    "id": 0,
    "input_files": {
      "nodes": "input/nodes.txt",
      "elements": "input/elements.txt"
    },
    "bounding_box" : [0.0, 50.0, 0.0, 50.0, 0.0, 50.0],
    "element" : {
      "type" : "Beam",
      "alpha" : 0.3,
      "beta"  : 1.0,
      "gamma" : 0.3,
      "tensile_strength" : 2000,
      "cohesion" : 4000,
      "friction_angle" : 0.0,
      "Emicro" : 2.0E+7,
      "distribution" : {
        "type" : "lognorm", 
        "sigma" : 1.0,
        "mu" : 1.0, 
        "min_threshold" : 0.2
      }
    },
    "reconnection" : {
      "status" : true,
      "threshold": 1.0E-03
    },
    "boundary_conditions" : [
      {
        "type" : "restrain",
        "node_set" : "-z",
        "restrain" : [true, true, true, false, false, false] 
      }
    ]
  },
  "analysis" : {
    "type" : "displacement",
    "loading" : {
      "node_set" : "+z",
      "disp" : [0.0, 0.0, 1.0E-04, 0.0, 0.0, 0.0],
      "dir" : 2,
      "face" : 5,
      "strain_node_set" : ["+z", "-z"],
      "max_steps" : 10000,
      "nreassemble_stiffness": 10,
      "max_threshold_lattices" : 100,
      "max_breakable_lattices" : 25
    }
  },
  "solver" : {
    "max_iterations" : 2000,
    "tolerance" : 1.0E-5
  },
  "post_processing" : {
    "output_steps" : 100,
    "results_path" : "results/"
  }
}
```

## Boundary node sets
Cartesian boundary node sets are automatically created based on the list of nodes. List of automatically created boundary node sets are: `-x`, `+x`, `-y`, `+y`, `-z` and `+z`.
