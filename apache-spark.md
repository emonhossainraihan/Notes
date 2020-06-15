**ApacheSpark:** Apache Spark is a unified analytics engine for large-scale data processing. 
Apache Spark is an open-source distributed general-purpose cluster-computing framework. 
Spark provides an interface for programming entire clusters with implicit data parallelism and fault tolerance.
Spark runs on Hadoop, Apache Mesos, Kubernetes, standalone, or in the cloud. It can access diverse data sources. 

**Quiz:**

- ApacheSpark programs when implemented in **R** are only slow when a lot of local executions withing R libraries takes place: 
Whenever data is copied from the cluster back to the driver (in this case an R script) and data processing is taking place locally 
using R libraries you might encounter performance problems
- Network Attached Storage (off-node storage approach): A remote relational database behaves basically like a remote file system. 
The only difference is that an additional data processing engine sits in between ApacheSpark and the data. 
But from a topological perspective this doesn't make a difference
- Data stored in RDDs is only read from the underlying storage systems when needed: This concept is called lazy evaluation and 
due to the inventors of functional programming languages. This means that functions are only executed if the output of their 
computations is needed for further downstream data processing

**Parquet Format:** Apache Parquet is a columnar storage format available to any project in the Hadoop ecosystem, 
regardless of the choice of data processing framework, data model or programming language.
- Binary Format 
- API for JVM/Hadoop & C++
- Columnar
- Encoded and Compressed 
- Machine-Friendly

