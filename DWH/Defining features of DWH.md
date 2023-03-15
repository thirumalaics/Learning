

### Subject-oriented:

- In operational systems, data is grouped and stored by individual applications
- datasets in operational systems are organized around individual applications in order to support them
- Inside these datasets, there will be data related to the different functionalities of the app
	- ex: datasets for an order processing app. These data sets provide data for all the functions of entering orders, checking stocks etc.
- data sets provide data for the specific apps to function
- In DWH, the data is grouped and stored by real-world **business subjects**, not by applications
- Business subjects are subjects that are critical to the enterprise.
	- for a manufacturing company: 1. Sales 2. Shipments 3. inventories

### Integrated:

- for proper decision making, the data should be complete
- pull relevant data from all sources
- source data resides in various dbs, files etc.
- data can be coming from internal and external sources.
- different sources mean difference in 
	- how they are represented(Schema)
	- how thery are stored(format)
- Since there are going to be inconsistencies due to differences above -> standardize before moving to DWH
 ![[Pasted image 20221230134758.png]]

some items that need standardizing:
- naming conventions
- character codes
- data attributes
- measurements

### Time-Variant Data:
- dwh has to store historical data
- every data structure in a dwh contains the time element
- time variant nature of data in a dwh allows us to:
	- analyze past
	- relates information to the present
	- forecast the future
### Non Volatile Data:
- dwh is not updated everytime some a small business action takes palce.
- data movement into dwh is periodic
- majorly dependent on the business requirement
- all datasets might not require the same frequency
- all business txns insert/update/delete to the operational system databases in real time
- we do not insert/update/delete the data in the dwh realtime
- data updates not a common activity in dwh
- data in dwh not as volatile as the data in the operations db

![[Pasted image 20230103143226.png]]


### Data Granularity:

- in operational systems, the data is kept at the lowest level of detail
- in a point-of-sale system at a grocery store, a txn is stored at the level of quantities of products involved in that transcation
- data stored in OpS is per transaction
- summary data not in OpS
- query results in dwh -> summarised ~ high level
- ex: sale of a product in India(highest level), sale of the same product in each state of India(one level down) etc...
- in dwh, it is efficient to keep data summarised at different levels
- analysis often goes from high level to low level
- data granularity in dwh refers to the level of detail
- lower the level of detail, higher the data granularity(lots of low level detail)
- if lower level detailed -> more storage needed
- data granularity decided based on the data types and expected system performance for queries

![[Pasted image 20230103144732.png]]


#DWHforITproff 