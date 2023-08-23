## Core Concepts
- not much important concepts due to the simplicity in design
- transformations and actions
	- same as in Structured API
	- there are some restrictions with respect to the transformations
		- this is usually because inability of Spark to prepare execution plans for some operations incrementally
	- whereas there is only one action available in SS, which is to start a stream
		- this will then run the computation continuously and o/p results

## I/p Sources
- several i/p sources for reading in a streaming fashion:
	- Apache Kafka
	- Files on a distributed file system like HDFS or S3 etc..
		- where Spark continuously reads new files that drop in the dir
	- a socket source for testing
- there are works to develop a source API which allows us to build our own streaming connectors

## Sinks
- destination of the result set of a stream
- Kafka
- almost any file format
- a foreach sink for running arbitrary computation on the o/p records
- a console sink for testing
- a memory sink for debugging

## Output Modes
- output modes determines how data is to be written to the data sink
- do we want to append new info or update rows as we receive more info, overwrite
	- similar to the write mode in structured API
	- the supported output modes are as follows:
		- append(only add new records to the output sink)
		- update(update changed records in place)
		- complete(rewrite the full output)
- there are some constraints when choosing an output mode
	- not all sinks support all output modes
	- and also not all queries support all output modes

## Triggers
- determines when data is output
- when structured streaming should check for new i/p data and update its result
- by default, SS will look for new i/p records as soon as it has finished processing the last group of i/p data
	- in turn providing the lowest latency possible
	- we should be mindful if this works for our case
		- in case of FS, multiple small files are written in this scenario
- spark supports processing time triggers, multiple triggers will be available in the future
## Windowing
- windowing is a process of logically grouping data to based on intervals of time
- at the end of each window the data processed data for that window is emitted as a default
- we can group data based on two different notions of time:
	- processing time 
	- event-time
- there are different windowing strategies, with the most intuitive being the fixed interval windowing
- a fixed interval of time is set for this windowing strategy
- these windows are non-overlapping
- for instance if the interval is one minute, in case of processing time, every data that arrives from 09:00 to 09:01(excluding) are grouped together in one window
## Event-time processing
- event-time means time fields that are part of our data
- we process it according to the time that it was generated, even if records arrive out of order at the streaming application due to slow uploads or n/w delays
- expressing event-time processing is simple in SS
- since the data is viewed as a table, we can perform operations using SQL
- based on the column which is our event-time field, SS can take some special actions including optimization and when it would be wise to forget the state
- the above actions can be controlled by the concept of watermarks

## Watermarks
- feature of streaming systems that allow us to specify how late we expect to see data in event time
- wm can also be used to control when to output a result for a particular event time window(waiting until the wm for it has passed)
