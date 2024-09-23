<h4>STRUCTURED APIs OVERVIEW : SQL, DATAFRAME and DATASETS</h4>

-------------------------------

Before we start demo the project, let’s talk about Spark’s Structured APIs!

These are a fundamental abstraction for writing most data workflows.

In Spark, you can write queries using **SQL, DataFrames, or Datasets**. Each of these formats has its own advantages and syntax, but they all share a common goal: **to manipulate and query data efficiently**.

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe49nlr4DuC-HNS4Sxh3e_GlKlr-V9u1YQe60t_oIaBmZW5tIGTOWTHHNrdcSzb8d5kbjD2mwMnKWJcNYfi1CQu-sdwdSmoijpVdiI5rO4KbhTOKb2v0JnqQlqqAPfcQJkw2TNvNoz6UDRlVvK2xVNDljs?key=Xvh8ngsTuNMqmEDFDBdPqg)



**CATALYST OPTIMIZER:** The reason we can use SQL, DataFrames, or Datasets is that the Catalyst Optimizer can "translate" and optimize any of these formats into an efficient **execution plan**. This allows you to choose the format that best suits your use case or preference without worrying about the nuances of physical execution.

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd2_8IMy2HVt5I6Md5LjNMfXbjdb5v6ZXhrNpMf4z7CpWfRaebRI9QdUqKoqwlGY6C0MEgnXm2TAlIFZop8u39kri_u6_j-xxor7dsKjzvwtM_kFwK5Q-BDtZVjPrB-3zSZ5yjoD7eviXUN7sc8CiRZUmD3?key=Xvh8ngsTuNMqmEDFDBdPqg)



*The **Catalyst Optimizer** is responsible for improving the performance of SQL queries and DataFrame operations through various optimization techniques.*



<h4>OVERVIEW OF STRUCTURED API EXECUTION  </h4>

-------------------------------

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcL3qWJq5HS5lZIesr9M28uskiK5JGewjGLXGrw7aFWv4GNmExvW7ZG9LDVU1gx7KXSyrbmybh1LSLAPu-QWa2TY-F4RSDtdcoB-qM5fN8Z_No_0c2oQa4ouIzpGVpy8D8LsIu6kTEX1A6eCtMLt029p261?key=Xvh8ngsTuNMqmEDFDBdPqg)



**1. Transformation into Logical Plan:** When you write code using any of these APIs (SQL, DataFrame, or Dataset), the code is initially transformed into an **abstract logical plan**. This logical plan represents the operations you want to perform but **does not specify how these operations will be executed physically** on the cluster, which is why it is called an **"unresolved logical plan**".:star:

**2. Catalog and Analysis:** Spark uses the catalog, a repository of all table and DataFrame information, to resolve **columns and tables** in the analyzer. The analyzer may reject the unresolved logical plan if the required table or column name does not exist in the catalog. If the analyzer can resolve it, the result is passed to the Catalyst Optimizer. :star:

**3. Catalyst Optimizer:** The Catalyst Optimizer then steps in to transform this logical plan into an optimized physical plan. The optimizer applies a series of rules and transformations to enhance query performance, such as reordering operations, eliminating redundancies, and selecting the best execution strategies. :star:

**4. Physical Plan:** After optimization, Catalyst generates a detailed physical plan that specifies exactly **how the operations will be executed** on the cluster. This includes decisions on how to read the data, which processing algorithms to use, and how to distribute the work across the cluster nodes.

**5. Execution and Results:** Finally, the physical plan is executed, and the result is returned to the user. Spark handles all the heavy lifting of execution and parallelism, ensuring that the query is processed in the most efficient manner possible. :star:

**EXECUTION:**

Upon selecting a physical plan, Spark runs all of this code over RDDs, the lower-level programming interface of Spark. Spark performs additional optimizations at runtime and finally returns the result to the user.



References:

:pushpin: [Spark-definitive-guide-Matei-Zaharia](https://github.com/VolodymyrGavrysh/My_RoadMap_Data_Science/blob/master/kds/books/Spark-The%20Definitive%20Guide.pdf)

