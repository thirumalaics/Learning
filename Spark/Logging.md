- two types
	- spark driver logs
	- executor logs
- driver log: contains job output
- executor logs: contains job executable or launcher output
	- ex: spark-submit submitted application xxx 
- the dataproc job driver(diff from spark driver), is a launcher for many job types
- when launching spark jobs, DJB runs a wrapper on the underlying spark-submit executable
- spark driver runs the job on the dataproc cluster in Spark client or cluster mode
- client mode: the spark driver run the job in the spark-submit process, Spark logs sent to DJB
- cluster mode the Spark driver runs the job in YARN YARN container. Spark driver logs are not available to the DJB
- 