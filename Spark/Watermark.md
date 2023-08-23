- for all stateful process that operates on event time, we must specify how much lateness we are expecting
	- this is specified via a watermark
	- so that the streaming application does not have to wait indefinitely for data of a window, all the while remembering the state
	- when this is the case for multiple windows, there might not be any free resources to process data of new windows
- wm: is an amount of time following a given event or set of events after which we do not expect to see any more data from that time
- def from 2.0: moving threshold in event time that trails behind the maximum event time seen by the query in the processed data. trailing gap: wm delay
	- there was no robust method available in DStreams API to handle late data
		- if an event occurred at a certain time but did not make it to the processing system by the time the batch for a given window started, it would show up in other processing batches
		- a given window's state or set of data is decoupled from a processing window
			- this means that as more events come in, structured streaming will continue to update a window with more information
- when specifying a wm of 10 mins:
	- any event that occurs more than 10 "event-time minutes" past a previous event should be ignored
	- also states that we expect to see every event within 10 mins
	- spark should remove intermediate state and, depending on the output mode, do something with the result
	- we need to specify watermarks because if we did not, we'd need to keep all of our windows around forever, expecting them to be updated forever
	- answer to the question: "how late do I expect to see data", will be the wm that we'll configure for our data
- specify wm using withWatermark method
- the method should be called before the groupBy()
- sS will continuously track the max observed value of the eventTime column and accordingly update the wm based on the wmgap that we provide
```
# in Python from pyspark.sql.functions import window, col
withEventTime\ .withWatermark("event_time", "30 minutes")\ .groupBy(window(col("event_time"), "10 minutes", "5 minutes"))\ .count()\ .writeStream\ .queryName("pyevents_per_window")\ .format("memory")\ .outputMode("complete")\ .start()
```
- any data that has a ts older than the wm is considered too late
- if windows are defined, all windows that have an end ts older than wm are closed -> rephrase with wm cross 

## Semantic guarantees with WM
- wm of 10m guarantees that the engine will never drop any data that is delayed by less than 10m compared to the latest event time seen in the i/p data
- data delayed by more than 10m is not guaranteed to be dropped -> may be part of the computation
	- whether it gets computed depends on the exact timing of when the record was received and when the micro-batch processing it was triggered

## Supported O/p modes
- aggs with time windows can use all three o/p modes
- there are some implications regarding state cleanup
- update: o/p only rows where the agg got updated
	- wming will ensure that the state will get cleaned up regularly
	- most useful and efficient
	- cannot use this mode to write aggs to append-only streaming sinks(file-based formats)
	- [Structured Streaming Programming Guide - Spark 3.4.1 Documentation (apache.org)](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html#handling-late-data-and-watermarking)
- complete:all updated aggs, irrespective of their age or whether they contain changes
	- can be used on all type of aggregations
	- for time window, aggs using complete mode means state will not be cleaned up even if a wm is specified
	- o/ping all aggs requires all past state
	- use with caution
- append
	- can only be used with aggs on event-time windows and wming enabled
	- for any agg without wms, every agg may be updated with any future data, and hence these cannot be o/p in append mode
	- we can write to append only sinks
	- dis: o/p will be delayed by wm duration - query has to wait for trailing wm to exceed the time window of akey before its agg can be finalized