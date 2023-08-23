Moreover, any business logic in your applications needs to work consistently across streaming and batch execution: for example, if you have a custom code to compute a user’s billing amount, it would be harmful to get a different result when running it in a streaming versus batch fashion! To handle these needs, Structured Streaming was designed from the beginning to interoperate easily with the rest of Spark, including batch applications. Indeed, the Structured Streaming developers coined the term continuous applications to capture end-to-end applications that consist of streaming, batch, and interactive jobs all working on the same data to deliver an end product. Structured Streaming is focused on making it simple to build such applications in an end-to-end fashion instead of only handling stream-level per-record processing.

- Spark includes two streaming APIs
- earlier DStream API in Spark Streaming is purely micro-batch oriented
- it has declarative(function-based) API but no support for event-time
- the second API is new and called Structured Streaming API
	- adds higher-level optimizations, event-time support and support for continuous processing

## DStream API
- based purely on Java/Python objects and functions
	- whereas in batch we had table-like abstractions such as DF and DS
- since the objects are language based, this limits the engine's opportunity to perform optimizations
- purely based on PT
- to handle event-time ops, apps need to implement on their own
- Dstreams only allows MB processing

## Structured Streaming
- higher-level streaming API
	- built on Structured APIs
- available in all environments where structured processing runs
- declarative API based on high level operations
- because SS built on structured API, SS can perform more types of optimizations automatically
- native support for event time data
- as of Spark 2.2, only mb model supported
- but continuous processing in works
- simplifies stream processing
- also designed to make it easy to build end-to-end continuous applications using Apache Spark that include streaming, batch , interactive queries
- SS does not use a separate API from DFs
- we simply write a normal df (or SQL) computation and launch it on a stream
- SS will automatically update the result of this computation in an incremental fashion as data arrives
- this is major help when writing end-to-end data applications: developers do not need to maintain separate streaming version of their batch code
	- possibly for a diff execution sys
	- decrease the risk of the codebases fall out of sync
- SS can o/p data to std sinks usable by Spark SQL, such as parquet tables
	- making it easy to query our stream state from another spark apps
- SS upgrade DS
- integration of ML with SS in progress
- treats a stream of data as a table to which data is continuously appended
- the job then periodically checks for new input data, process it, updates some internal state located in a state store if need and updates its result
- main advantage: we do not have to change our Query's code when doing a batch or stream
	- we should only have to specify whether to run that query in a batch or streaming mode
- SS takes care of the incrementalization of our query
	- i.e. update its result efficiently whenever new data arrives, and will run it in a fault tolerant fashion
- there are some limits to the types of queries structured streaming will be able to run
	- we must also think of some new concepts that are specific to streaming
	- such as event-time and out-of-order data
- by integrating with the rest of Spark, ss enables users to build what we call continuous applications
- continuous application: is an end-to-end application that reacts to data in real time by combining a variety of tools: streaming jobs, batch jobs, joins b/w streaming and offline data and interactive ad-hoc queries
- because most streaming jobs today are deployed within the context of a larger continuous application, Spark makes it easy to specify the whole application in one fw and get consistent results across these diff portions of it