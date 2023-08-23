- involves reading from at least one unbounded source, meaning the data is unbounded without any beginning or end
- data that keeps arriving is continuously considered in the computation of the result
- the normal batch processing involves reading the fixed-size data completely and follow the read process with computations
	- the results are generated once for a run
- in case of unbounded source, as the data does not have a predetermined end, the processing system cannot wait for the entire data to arrive in order to process
	- to tackle the problem that unbounded source systems poses, stream processing systems logically group elements into fixed-interval windows based on the time that they were created(in the source system)/received(in processing system)
		- this might not be the case always
- so there will be much more outputs for the same computation
	- as the results are computed for each window or logical groups
- there are cases where the stream processing might include joining the stream data with a batch data, even then the job which performs this operation is a streaming job

## Stream Processing Use Cases
- stream processing is necessary in situations where time is of the essence
	- wherever there is an immediate need for processed data, within a short time that the source data arrives, an implementation of stream processing is required
- notifications and alerting
- real time reporting
	- ex: reporting dashboards, usage of new features as they are rolled out
- incremental ETL
	- into the DWH
	- where queries need recent data
- real time decision making
	- bank wants to automatically verify whether a new txn on a customer's cc represents fraud based on their recent history, and deny the txn if the charge is determined fradulent
		- this decision needs to be made real-time while processing each txn

## Advantages of Stream Processing
- stream processing enables lower latency
	- so applications that need to respond quickly can benefit from this advantage
	- whereas batch processing provides us with higher throughput than many systems
- can also be more efficient in updating a result than repeated batch jobs, because it automatically incrementalizes the computation
	- really helpful when we are dealing with high velocity data
	- implementing incremental logic in stream processing is almost effortless
	- to achieve the same performance with batch, we would have to implement the incremental logic by hand and from scratch

## Challenges of stream processing
- processing out-of-order data based on application timestamps(event time)
	- this becomes an issue when the business logic that we have depends on the order of the data
	- for ex: we might want to take some action based on the pattern of values for a column
		- because we look for a pattern, order becomes important
- maintaining large amounts of state
	- state is intermediate data stored based on the records or events that had already arrived
	- state is common in aggregations, where to find the aggregation sum for one row, the processing system has to store intermediate data from many records. finally the state is emitted as the aggregation result when a particular condition is met(which is usually in terms of time)
	- maintaining state becomes necessary in many operations
	- because of the widespread presence of operations that store state, these operations are collectively called as stateful operations
- supporting high-throughput data
	- handling and supporting high-throughput data is one of the biggest challenges posed to stream processing
	- one common solution is to process the data for shorter intervals
	- this also poses a challenge of handling load imbalance and stragglers
- processing each event exactly once despite machine failures ?
- responding to events at low latency
- joining with external data in the other storage systems
- the processing sys should also determine how to update/write to o/p sinks transactionally as new events arrive

## Designing Stream Processing 
- to tackle all the challenges described above there are multiple designs

## Record-at-a-Time versus Declarative API
- simple
- just pass each event to the application and let it react using custom code
- this was the approach followed in the earlier streaming systems and it is used when applications need full control over the processing of data
- streaming that provide this kind of record-at-a-time API just give the user a collection of plumbing to connect together into an application
- downside of this approach is that operations such as maintaining state  are solely governed by the app
	- ex: with a record-at-a-time API, we are responsible for tracking state over longer time periods, dropping it after some time to clear up space, and responding differently to duplicate events after a failure
	- since we have most of the control, dealing with all the problems stated above needs individual expertise
- as a result many newer streaming sys provides declarative APIs. where our app specifies what to compute but not how to compute it in response to each new event and how to recover from failure

## Event time v Processing Time
- for streaming systems, one of the concerns is whether the streaming system natively supports event time
- event time processing processes data based on the timestamps present as part of the record
- processing time processing processes data based on the time when the data was received at the processing system
- whenever we use event time, records may arrive to the systmer out of order
- when it is important for us to process data based on the order that they were created, event time processing is the way to go
- event time processing requires careful state tracking, as there are huge chances of out-of-order data
	- there should be a standard way to determine when all the elements might have been received
	 - based on this, that state will be cleared

## Continuous Versus Micro-batch exectuion
- continuous processing-based systems:
	- each node in the system is continually listening to messages from other nodes and outputting new updates to its child nodes
	- if we are performing map-reduce over an input stream, node that performs map reads records one by one from an input source, computes its function on them, and send them to the node that performs the reduce
	- the reducer would then update its state whenever it gets a new record
	- this keeps happening for each record 
	- as perceivable, this approach leads to the least possible latency, because each message is processed immediately without waiting much for any other record to arrive
	- but disadvantage is that the overall throughput is low as the entire workflow is performed on a record basis
- This could have been prevented if the records were batched and the workflow were to be performed on the entire batch, which is the stream processing design that micro-batch systems follow
- micro-batch sys:
	- they wait for a small interval(differs between workloads) of time to collect small batches of input data
	- then each batch is processed in parallel, similar to the execution of a batch job in Spark
	- because the same optimizations that are applicable to batch systems can be applied in this case, micro-batch systems achieve high throughput
	- because we are batching, each node processes a batch and hence the number of records processed per node per time is higher
	- the problem with the micro-batching approach is it's latency as each record has to wait for some time before being processed
	- large-scale streaming applications might prefer micro-batches because the input data arrives at a high-throughput
	- in this case if we opt for record-by-record, then the backlog of records could increase exponentially
	- latency 100ms to 1 second, depending on the application
	- to acheive the same throughput as the record-by-record approach, micro-batches require fewer nodes
- deciding b/w these approaches, key considerations: total cost of operation and latency