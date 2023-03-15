| Data Warehouse                  | Data Mart                   |
| ------------------------------- | --------------------------- |
| Enterprise wide                 | Departmental                |
| Union of all datamarts          | A single busiess process    |
| Data recieved from staging area | STARjoin (facts&dimensions) |


Two ways of building a DWH:
1. overall dwh feeding the dependent datamarts (top down)
2. several departmental or local dms combining into a dwh (bottom-up)

### Top-Down Approach:

- in this approach data stored in the lowest leve of granularity

##### Advantages:
	1. Enterprise view of the data
	2. inherently architected, not a union of different datamarts
	3. centralised rules and control
	4. may see quick results if implemented with iterations

##### Disadvantages:
	1. Takes longer to build even with iterative method
	2. Risk of failure is high
	3. needs high level of cross functional skills
	4. high outlay without poc


### Bottom-up Approach:

- key consideration is [[conforming the dimensions]]
- conformed dimension is a dimension that has the same meaning to every fact with which it relates - from tech target
- in this approach, dms are created first to give analytics and reporting for specific business subjects
- Dms contain data at the lowest level of granularity and also summaries depending upon the need for analysis
- these dms are joined or unioned together by [[conforming the dimensions]]
##### Advantages:
	1. Faster and easier implementation
	2. Favourable return on investment and proof of concept
	3. Less risk of failure
	4. Inherently incremental; can schedule important data marts first
	5. Allows individual teams to grow simultaneously
##### Disadvantages:
	1. Each datamart has its own narrow view of data
	2. Spreads redundant data in every data mart
	3. proliferates unmanageable interfaces
	4. perpetuates inconsistent and irreconcilable data
- each dm will be blind to overall requirements of the entire organization



#DWHforITproff 