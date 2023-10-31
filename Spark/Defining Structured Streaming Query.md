
- step1: define input sources
	- define a df from a streaming source
	- the handle is DataStreamReader
	- the handle to read streaming sources is available withing SparkSession as spark.readStream
	- has most of the same methods as DataFrameReader
	- when we define a dataframe with the streaming source, the data is not immediately read
	- necessary configurations are set up to later read the data
		- the reading process starts for the first time only when the streaming query is started, more on this will be covered later in the article
	- it is important to remeber, a streaming query can define multiple input sources, including batch and streaming
- Step 2: transform data
	- dataframe operations can be applied
	- the resulting dataframe is also a streaming dataframe
	- most of the operations applicable to a batch df are also applicable to the streaming df
	- to understand which operations are supported in SS, we have to recognize two classes of data transformations:
	- stateless:
		- operations that do not require any information from previous rows to process the next row
		- each row can be processed by itself
		- the lack of previous state makes them stateless
		- can be applied to both batch and streaming without any restrictions
	- stateful:
		- operations that require some information from the previous set of rows to process the current row
		- any grouping, joining or aggregating operations are stateful
		- many of these operations are supported
		- some combinations are not hard or impossible to incrementalize
- define o/p sink and o/p mode
	- done with the help of the handle DataStreamWriter
	- available within streaming datafram: df.writeStream
	- has additional methods to specify
		- o/p writing details(sink and o/p mode)
		- processing details(how to process data and how to recover from failures)
		- all the words seen till now and their counts are chosen to be written into the console sink
- specifying processing details
	- can be specified using DataStreamWriter
	- this can include the trigger to display our results
	- a new configuration called as checkpoint location:
	- location to store streaming query progress info
		- includes details of till where the processing is complete
		- if the query fails and if restarted, the query starts from where it left of with the help of this checkpoint info
- start the query
	- once the above steps are done, the final step is to start the query
	- the method returns an instance of streamingQuery
		- it represents an active query and this object can be used to manage the query
		- the method start is non-blocking, so the control is returned to the calling module
			- and hence the rest of the code in the module can be executed
		- but if that is not what we are looking for, we can apply the awaitTermination() method on the streaming query
			- this will make sure the main thread is blocked and only when the streaming query is terminated, the control comes back to the main thread
			- if the streaming query fails due to some error, then awaitTermination also fails with the same exception
			- the method takes one argument which is the timeoutMillis, defining the a timeout duration
	- just like we were able to explicitly start a query, we can explicitly stop a query