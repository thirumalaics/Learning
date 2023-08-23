- to understand this, we need to understand the two core resources at play
	- node-to-node communication strat
	- per node computation strat
- understanding this might help us optimize our jobs
- joins usually trigger a large amount of data movement across Spark executors
- five join strategies by which spark implements a join transformation
	- two of them widely used

## Communication strategies
- two types of cluster comms during join
	- all-to-all comms
		- during shuffle join
	- broadcast join
- the above types are abstract and common
	- because there might be many changes because of improvements to cost based optimizer and improved comms strategies
- the following explanation takes a simplified view that we will have either a big or small table
	- if medium sized table, things happen differently
#### Big table-to-big table
- when this is the case: shuffle join
![[Pasted image 20230602185518.png]]
- in shuffle join, every node talks to every other node
	- they share data according to which node has a certain key or set of keys
	- these joins are costly
		- because the n/w can become congested with traffic
			- especially if our data is not partitioned well
- every worker node and potentially every partition will need to comms with one another during the entire join process
	- if no intelligent partitioning of data
- Shuffle sort merge join
- efficient for merging two large datasets over a common key that is
	- sortable
	- unique
	- can be assigned or stored in the same partition
		- two datasets with a common hashable key that end up being on the same partition
		- this means that all rows within each data set with the same key are hashed on the same partition on the same executor
		- this means that data has to be colocated or exchanged b/w executors
- This join scheme has two phases: sort phase followed by a merge phase
- the sort phase sorts each data set by its desired join key
- the merge phase iterates over each key in the row from each data set and merges the rows if the two keys match
- by default SSMJ is enabled via `spark.sql.join.preferSortMergeJoin`

#### Optimizing the SSMJ
- we can eliminate the shuffle step from this scheme if we create partitioned buckets 
	- for common sorted keys
	 - columns on which we want to perform frequent equi-joins
- we can create an explicit number of buckets to store specific sorted columns 
	- one key per bucket
- presorting and reorganizing data in this way boosts performance
- the performance enhancement is more evident when the table which was optimized using the above technique is part of multiple join operations
#### When to use a shuffle sort merge join
- when each key within two large datasets can be sorted and hashed to the same partition by Spark
- when we want to perform only equi-joins
## Big table-to-small table
- brodcast hash join aka map-side-only join
- when the table is small enough to fit into the mem of a single worker, with some breathing room ofc
	- we can optimize our join
	- we can still use the above strat
	- but more efficient to use broadcast join
	- replicate our small dataframe onto every worker node in the cluster
		- assuming 
			- one small df, small enough to fit in the driver's and executor's memory
			- another large enough to ideally spared from movement
	- this prevents us from performing all-to-all comms during the entire join process
	- perform broadcast once at the beg and then let each individual worker node perform the work without having to wait or comms with any other worker node
![[Pasted image 20230602190230.png]]
- at the beginning, there will be a large comms
- after that every node works alone
	- making CPU the biggest bottle neck
- by default spark will use broadcast join if the smaller data set is less than 10MB
	- `spark.sql.autoBroadcastJoinThreshold`
		- set to -1, to ban this join
		- tune it depending on how much mem we have on each executor and in the driver
- fastest and easiest join that spark offers
	- considering shuffling as an important aspect, here no shuffling
- go for it if 
	- each key within the smaller and larger ds is hashed to the same partition by Spark
	- size
	- when we want equi-join,to combine two datasets based on matching unsorted keys
		- beacuse, if not equi-joins, the join might require the data from other paritions
			- ex: > condition
	- when we are not worried about excessive n/w b/w usage or OOM errors
- if we try to broadcast something too large, we can crash our driver node
- we can explicitly give the optimizer a hint that we would like to use a broad cast join by using the correction function around the small df
![[Pasted image 20230605141247.png]]
- SQL interface also includes hints
	- but these are not enforced
	- optimizer might choose to ignore them
![[Pasted image 20230605141341.png]]
## Little Table-to-Little table
- best to let Spark decide how to join them
- but we can always force a broadcast join