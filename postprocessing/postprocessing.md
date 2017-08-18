# Post processing

* Lattice Element Method (LEM) exports forces, displacements, broken lattices as compressed binary VTP files, which can be visualised using [ParaView](http://www.paraview.org/) or [Visit](https://wci.llnl.gov/simulation/computer-codes/visit/).

* A summary statistics for the test is exported at every step to a SQLite database. Each analysis has a unique id (uuid), type of analysis and a time stamp. To view the uuid of analysis: `sqlite3 ./stats.db "select * from Analyses";`. 

```
id|type|timestamp
b09b5957-4560-4c46-a3cc-c24f1552fefa|Uniaxial displacement controlled test|2017-03-28 12:47:10
f80a5167-1f30-44e1-b307-6a971a63e0c1|Uniaxial displacement controlled test|2017-03-28 12:48:46
5a6dd738-9bca-4513-a18b-afbeb817916b|Uniaxial displacement controlled test|2017-03-28 12:49:48
```

* The summary of the analysis is written to `Stats` table in the database. This can be viewed by running: `sqlite3 -header ./stats.db "select * from Stats where analysis='b09b5957-4560-4c46-a3cc-c24f1552fefa';"`

```
analysis|timestep|pressure|strain|nthresholdlattices|nbrokenlattices|nunstablenodes|accumstrainenergy
b09b5957-4560-4c46-a3cc-c24f1552fefa|0|1756.54813805836|8.74184279022203e-05|0|0|0|0.0
b09b5957-4560-4c46-a3cc-c24f1552fefa|1|1858.25934081275|9.24803110498628e-05|9|9|0|0.398622200104824
b09b5957-4560-4c46-a3cc-c24f1552fefa|2|1858.18819035461|9.24813518538412e-05|3|3|0|0.598511020319855
b09b5957-4560-4c46-a3cc-c24f1552fefa|3|1870.62270044566|9.31027055645156e-05|5|5|0|0.912926929056728
b09b5957-4560-4c46-a3cc-c24f1552fefa|4|1883.13007276794|9.37289192437613e-05|6|6|0|1.56735984822119
```

* Export SQLite to CSV `sqlite3 -header -csv ./stats.db "select * from Stats where analysis='b09b5957-4560-4c46-a3cc-c24f1552fefa';" > summary.csv`


* To extract nodal displacements for a list of nodes, specify the nodes in the post-processing section of the JSON file:

```json
  "post_processing" : {
    "output_steps" : 5,
    "results_path" : "results/",
    "nodes" : [0, 242]
  }
```

* To query for nodal displacement (disp2) in `sqlite`, open the database using `sqlite3`: `sqlite3 ./stats.db`. In `sqlite3` run the following command in `sqlite>`: `select timestep, disp2 from Nodes where analysis="b09b5957-4560-4c46-a3cc-c24f1552fefa" and nid=0;`, this will extract timestep and displacement2 (z) for node 0 and specified analysis.

```
timestep|disp2
0|0.00354388962268145
1|0.00374898244681804
2|0.00374914990973956
3|0.00377445166430338
4|0.00379986001471253
```

* To combine results from multiple tables, for example combine displacement of nodes to results from stats: `select t1.*,t2.disp2 from (select * from Stats where analysis="b09b5957-4560-4c46-a3cc-c24f1552fefa") t1 inner join (select * from Nodes where analysis="b09b5957-4560-4c46-a3cc-c24f1552fefa" and nid=0) t2 on t1.timestep=t2.timestep`;

```
analysis|timestep|pressure|strain|nthresholdlattices|nbrokenlattices|nunstablenodes|accumstrainenergy|t2.disp2
b09b5957-4560-4c46-a3cc-c24f1552fefa|0|1756.54813805836|8.74184279022203e-05|0|0|0|0.0|0.00354388962268145
b09b5957-4560-4c46-a3cc-c24f1552fefa|1|1858.25934081275|9.24803110498628e-05|9|9|0|0.398622200104824|0.00374898244681804
b09b5957-4560-4c46-a3cc-c24f1552fefa|2|1858.18819035461|9.24813518538412e-05|3|3|0|0.598511020319855|0.00374914990973956
b09b5957-4560-4c46-a3cc-c24f1552fefa|3|1870.62270044566|9.31027055645156e-05|5|5|0|0.912926929056728|0.00377445166430338
b09b5957-4560-4c46-a3cc-c24f1552fefa|4|1883.13007276794|9.37289192437613e-05|6|6|0|1.56735984822119|0.00379986001471253
```

## Lattice table

* Stiffness and status of lattices are written to a table called `Lattices`:

```
analysis|timestep|eid|stiffness|status
b09b5957-4560-4c46-a3cc-c24f1552fefa|0|0|16587793.8489298|1
b09b5957-4560-4c46-a3cc-c24f1552fefa|0|1|8789304.42281428|1
b09b5957-4560-4c46-a3cc-c24f1552fefa|0|2|1397908.18070347|1
b09b5957-4560-4c46-a3cc-c24f1552fefa|0|3|11197073.256249|1

```

* To extract stiffness data of a particular time step `n`:
 
```
 select * from Lattices where analysis='b09b5957-4560-4c46-a3cc-c24f1552fefa' and timestep=0;
```

> **Info** A status of 1 indicates active, and 0 otherwise.s





