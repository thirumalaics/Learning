## Spark Driver
- process 
- controller of the execution of a Spark App
- maintains state of the Spark cluster
	- state of executors
	- state of the tasks assigned to the executors
- interfaces with the cluster manager in order to get physical resources and launch executors

## Spark Executor
- process
- performs the tasks assigned by the driver
	- reports back the status of the tasks
	- returns results

## Cluster manager
- maintains the cluster of machines in which our apps run
- cluster manager will have its own driver(sometimes called the master) and worker abstractions
- core difference is that these are physical resources and where as in spark they are processes
![[Pasted image 20230323174922.png]]

## Execution modes
- gives us the power to determine where the above resources are physically located
- three modes available
- normal rectangle - > driver processes
- dotted rectangle - > executor processes
### Cluster mode
- we submit the precompiled code(java) or Py script to  CM
- then the CM launches the driver process on a worker node inside the cluster + executor processes
![[Pasted image 20230323175602.png]]
- CM is responsible for maintaining all spark app related processes

### Client mode
- almost same as Cluster mode
- Spark driver remains on the machine that submitted the app
- Client machine responsible for maintaining the driver processes
- CM responsible for maintaining the exec processes
![[Pasted image 20230323175910.png]]
- machine on the top left called as gateway machines or edge nodes

## Local mode
- runs the entire spark app in a single machine
- acheives parallelism through threads on that single machine