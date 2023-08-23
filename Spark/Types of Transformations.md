
Now that we know there are two types of operations in Spark and one of them is Transformations, let's dive deeper into the types of Transformations. Transformations are operations that definitely need optimizations when working with large datasets. When we chain multiple transformations together, since transformations are lazily evaluated, Spark can analyse optimised way of executing them. 
Transformations can be classified into two types:
1. Narrow
2. Wide

## Narrow Transformations:

