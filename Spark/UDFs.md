- to write our own custom transformations using Py or scala and even external lib
- UDFs can take and return one or more columns as i/p
- UDFs can be written in several diff programming langs
	- do not need to create them in an domain-specific language
- they are functions that operate on Data record by record
- by default functions are registered and scoped to be used in a specific SparkSession or Context
- there are performance considerations when it comes to writing UDFs
- to understand how the performance is affected when using a UDF, it is important to understand what exactly happens when we create a UDF, pass that into Spark, and then execute code using that UDF
- as a first step, we are supposed to write a function which will take necessary number of input columns and perform the operations and return the column(s) #check
- in the above figure, the function has been tested with sample input values and they work perfectly fine on values
	- so we can expect them to work as required on individual row values
- now to actually use this function with dataframes, tables or views, we need to register the function with Spark so that we can use them on all of our worker machines
- after registering, Spark will serialize the function on the driver and transfer it over the n/w to all executor processes
	- this happens regardless of the language we use to express our logic
- when we use the function, there are two different things that occur
	- if the fn is written in Scala or Java, we can use it within the JVM
	- this means that there will be little performance penalty aside from the fact that we cannot take advantage of the code generation capabilities that Spark has for built-in fns
	- there can be performance issues if we create or use a lot of such objects
		- check ch19
- if the fn is written in py, what happens is different from the aforementioned case
	- Spark starts a python process on the worker, serializes all of the data to a format that Python can understand
	- executes the function row by row on that data in the py process
	- finally returns the results of the row operations to the JVM and Spark
[Performance in Apache Spark: Benchmark 9 Different Techniques | by David Vrba | Towards Data Science](https://towardsdatascience.com/performance-in-apache-spark-benchmark-9-different-techniques-955d3cc93266)
[Spark. From understanding core concepts to… | by Sanjay Singh | Towards Data Science](https://towardsdatascience.com/spark-71d0bc25a9ba#:~:text=Spark%20%26%20Java%20Virtual%20Machine,computer%20where%20it%20is%20running.)
![[Pasted image 20230619091645.png]]
- even though starting the python process is expensive, the real cost is in serializing the data to python
- this is costly for two reasons: 
	- it is an expensive computation
	- after the data enters Python, Spark cannot manage the memory of the worker
	- this means that there are chances for a worker to fail if it becomes resource constrained
		- this might be the case because both JVM and python are competing for memory on the same machine
	- so the recommendation is that write the UDFs in scala or Java - we can still use the function from python
		- small efforts, significant speed ups
- once the function is written, it has to be registered with Spark
- to only use it as a dataframe API function, we can call the udf function with our function as a argument, this registers the function with Spark
- the UDF method return a function which can be used only on column(s) of a dataframe
- one more thing to note is that the return value of the udf when applied to column is by default StringType
	- we can configure this by providing an extra argument to the UDF function called `returnType`, the return type can be set to any Spark internal type
	- to be more precise, the value for the argument can be either an object of [`pyspark.sql.types.DataType`](https://spark.apache.org/docs/3.1.3/api/python/reference/api/pyspark.sql.types.DataType.html#pyspark.sql.types.DataType "pyspark.sql.types.DataType") or a DDL-formatted type string
`df.select(udf_power(col('id'))).printSchema()`
![[Pasted image 20230619093758.png]]
```
from pyspark.sql.functions import *
def power(col):
    return col**2
udf_power = udf(power)
df.select(udf_power(col('id'))).show()
```
![[Pasted image 20230619093933.png]]

- we cannot use the above function within a string expression or in SparkSQL, if we want to do so, we have to register the function as a SparkSQL function
```
spark.udf.register('power2', power,IntegerType())
```
- one important thing to note here is that the return type we specify while registering the UDF should be compatible with the type of the object returned by the function itself
- for example, if the function registration is as follows:
`spark.udf.register('power2', power,DoubleType())`
- the return type given during the registration is not compatible with the object that is actually returned
- the object returned by the function is of type int, Spark will not be performing any casting to the given internal type `DoubleType()`
	- it will not even throw an error but will just return null to indicate that there was a failure 
	- instead the values for the returned column are all nulls
- to be more specific with the above use case: 
	- the range function creates a column of type IntegerType
	- when the function is called on each row, the equivalent type in python is int and the function applies the square. The result of square operation is also int. 
	- but for the registered DoubleType, the result should have been it's python equivalent float. Since this is not the case, there is a incompatibility of types which results in a column of Nulls.
- when we want to optionally return a value from a UDF, we should return None in Python
- 