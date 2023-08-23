- dataframe operations are converted into a logical plan
	- abstract representation of the computation
	- this plan is used by Spark SQL to plan execution of a streaming query
1. analyze and optimize the logical plan to ensure that it can be executed incrementally and efficiently on streaming data
2. SparkSQL starts a background thread that continuously executes the following loop(only applicable to queries that use micro-batch based trigger modes such as ProcessingTime and Once)
	1. based on the configured trigger interval, the execution thread checks the streaming sources for the availability of new data
	2. if available, the new data is processed by running a micro-batch. 
		- From the optimized logical plan, an optimized Spark execution plan is generated that reads the new data from the source. Incrementally computes the updated result, and writes the output to sink according to the configured output mode
	3. For every micro-batch, the exact range of data processed(ex: the set of files or the range of Apache Kafka offsets) and any associated state are saved in the configured checkpoint location
		- so that the query can deterministically reprocess the exact range if needed
3. this loop continues until the query is terminated, which can occur for one of the following reasons:
	- a failure has occurred in the query(either a processing error or a failure in the cluster)
	- the query is explicitly stopped using streamingQuery.stop()
	- if the trigger is set to Once, then the query will stop on its own after executing a single micro-batch containing all the data