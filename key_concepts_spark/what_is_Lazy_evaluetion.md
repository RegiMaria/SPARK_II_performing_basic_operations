<h3> Lazy evaluation </h3>

In Spark, instead of **modifying data immediately** when you perform an operation, you **build a plan of transformations** that you want to apply to your source data. By waiting until the last moment to execute the code, Spark compiles this plan—from your initial DataFrame transformations to an optimized physical plan that will run as efficiently as possible across the cluster. This provides immense benefits, as Spark can optimize the entire data flow from end to end.



:heavy_check_mark: **Narrow Transformations**: These transformations create an execution plan but do not immediately change the data. They are applied lazily, meaning Spark only prepares the execution plan without actually performing the operations until an action is called.

Examples include:

- `filter()`
- `select()`
- `withColumn()]`



:heavy_check_mark:**Actions**: These are operations that trigger the actual execution of the transformations. Only when an action is called does Spark execute the** transformation plan and process** the data as defined.

Examples of actions:

- `count()`
- `show()`
- `collect()`
- `join()`
- `groupBy()`
- `reduceByKey()`

Lazy evaluation allows Spark to compile and optimize the entire execution plan before actually processing the data, which can lead to significant improvements in efficiency and performance.

(ZAHARIA, pg. 27)