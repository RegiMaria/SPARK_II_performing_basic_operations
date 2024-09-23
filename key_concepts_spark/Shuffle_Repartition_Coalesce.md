<h3> Shuffle, Repartition, Coalesce </h3>

--------------------------------

As we know, the Apache Spark was created to solve the problem of processing large volumes of distributed data, so it’s important to understand some key concepts that play a crucial role in optimizing data processing and can directly impact processing time.

**Key points:**

- Narrow transformation
- Wide transformation
- Repartition
- Coalesce
- Shuffling



**Summary of key points covered in this project:**

1. **Transformations and Actions in Spark**:
   - **Transformations**: Operations that don’t run immediately but define a plan for execution. Examples include `select()`, `withColumn()`, `filter()`. Spark only runs transformations when an action is called.
   - **Actions**: Operations that execute the transformation pipeline and return a result. Examples include `count()`, `collect()`, `show()`.
2. **Narrow vs. Wide Transformations**:
   - **Narrow Transformations**: Transformations that can run within a single partition without moving data between partitions. Example: `map()`.
   - **Wide Transformations**: Transformations that require moving data between partitions (shuffling). Example: `groupBy()`, `join()`, `reduceByKey()`.
   - **Why minimize wide transformations?**: Wide transformations are costly because they move data over the network, increasing processing time and resource usage.
3. **Shuffling and Performance Impact**:
   - **Shuffling**: Occurs when Spark needs to reorganize data between partitions, causing significant overhead due to moving data across nodes in the cluster.
   - **How to minimize shuffling?**: Avoid operations that cause shuffling, such as unnecessary joins and groupBys, use techniques like `broadcast join`, or control the number of partitions with `repartition()` and `coalesce()`.
4. **Performance Optimization**:
   - **Early Filtering**: Apply filters (`filter()`) as early as possible in the pipeline to reduce the amount of data processed.
   - **Column Selection**: Use `select()` to work with only the necessary columns, reducing the volume of data processed and transferred.
   - **Smart Partitioning**: Partition data to enable efficient reading and reduce the need to read irrelevant data.
   - **Cache/Persist**: Reuse data in memory (using `cache()` or `persist()`) when the same data will be used repeatedly in different stages.

Performance optimization should be considered at all levels (job, stage, task) to have a positive impact on the production environment.



**REFERENCES:**

:pushpin:[Comprehensive Guide to Optimize Databricks, Spark and Delta Lake Workloads](https://www.databricks.com/discover/pages/optimize-data-workloads-guide) 

:pushpin:[What is the difference between a Narrow Transformation and Wide Transformation](https://community.databricks.com/t5/data-engineering/what-is-the-difference-between-a-narrow-transformation-and-wide/td-p/22117) 

:pushpin:[What Is Data Transformation?](https://www.databricks.com/glossary/what-is-data-transformation)