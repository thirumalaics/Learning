- Spark refers to the Spark JVM implementation as a whole
- PySpark refers to the Python API for Spark
- Spark is a system that runs on the JVM(usually) across multiple machines
	- PySpark enables direct control of and interaction with this system via Python
- Data processing is handled by Python processes
- data persistence and transfer is handled by Spark JVM processes
- Python driver program communicates with a local JVM running Spark via Py4J
	- Spark workers spawn python processes, communicating results via TCP sockets

[Python, Spark and the JVM: An overview of the PySpark Runtime Architecture - DEV Community](https://dev.to/steadbytes/python-spark-and-the-jvm-an-overview-of-the-pyspark-runtime-architecture-21gg)