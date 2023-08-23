- to control when data is output to our sink
- can also be thought of as a way to ensure that we do not overwhelm our o/p sink with too many updates or to try and control file sizes in output
- at this point in time there is only one periodic trigger available
	- based on processing time
- once trigger to run a processing step once
- more trigger will likely be added in the future

## Processing time trigger
- we provide duration as a string
- this trigger will wait for multiples of the given duration in order to output data
- if a trigger is missed because the previous processing has not completed, then spark will wait until the next trigger point(i.e. next minute) rather than firing immediately after the previous processing completes

## Once Trigger
- useful in both dev and prod
- will only output once
- during dev, we can test our app on just one trigger's worth of data at a time
- in prod, it can be useful to run our job manually or ad-hoc
- this might be helpful while reading data from fs, we do not have write code to exclude reading files from the last run because SS maintains the files that had been read in the checkpoint loc

## Continuous Processing
- new experimental execution mode introduced in Spark 2.3 that enables low (~1ms) end-to-end latency with at-least-once fault-tolerance guarantees
- where as micro-batch processing engine achieves exactly-once guarantees but achieves latencies of 100ms at best
- includes specifying a continuous trigger with the desired checkpoint interval as a parameter
	- the interval determines the time after which the streaming query checkpoints the progress
- the resulting checkpoints for all these triggers are compatible with each other, a query that used continuous trigger can be modified and restarted with a different trigger
- this mode comes with restrictions and only certain type of queries are supported
	- map-like df/ds operations
	- no agg functions
	- deterministic using time are challenging
- CP launches multiple long-running tasks that continuously read data from sources, process it and continuously write to sinks
- the number of tasks required by the query depends on how many partitions the query can read from the sources in parallel
	- make sure enough cores are present
	- if reading for a Kafka topic with 10 partitions, the cluster must have at least 10 cores for the query to make progress
- no automatic retries of failed tasks, restart manually