# Post processing
=================

* Lattice Element Method (LEM) exports forces, displacements, broken lattices as VTP files, which can be visualised using [ParaView](http://www.paraview.org/).

* A summary statistics for the test is experted at every step to a SQLite database. Each analysis has a unique id (uuid), type of analysis and a time stamp. To view the uuid of analysis: `sqlite3 ./stats.db "select * from Analyses";`. 

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






