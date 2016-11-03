Preprocessing
=============

Lattice Element Method (LEM) code use JSON file type for configuring the input.

The mesh configuration defines id, input files, bounding box, element, node sets and boundary conditions:

```json
  "mesh": {
    "id": 0,
    "input_files": {
      "nodes": "input/nodes.txt",
      "elements": "input/elements.txt",
      "fracture_elements": "input/fracture_elements.txt"
    },
    "bounding_box" : [0.0, 50.0, 0.0, 50.0, 0.0, 50.0],
    "element" : {
      "type" : "Beam",
      "Emicro" : 2.0E+7,
      "alpha" : 1.0,
      "beta"  : 1.0,
      "tensile_strength" : 2000,
      "cohesion" : 2000,
      "friction_angle" : 0.0
    },
    "node_sets" : {
      "top" : "input/top_nodes.txt",
      "bottom" : "input/bottom_nodes.txt"
    },
    "boundary_conditions" : [
      {
        "type" : "restrain",
        "node_set" : "bottom",
        "restrain" : [false, false, true, false, false, false] 
      }, 
      {
        "type" : "displacement",
        "node_set" : "top",
        "disp" : [0.0, 0.0, 0.00425, 0.0, 0.0, 0.0]
      }
    ]
  }
```
