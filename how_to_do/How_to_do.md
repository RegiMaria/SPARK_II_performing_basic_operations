<h3>  How to do </h3>

:pushpin:The code is here

:heavy_check_mark: **About Data Ingestion**

In the book [*Understanding ETL*](https://www.databricks.com/resources/ebook/understanding-etl?scid=7018Y000001Fi0cQAC&utm_medium=paid+search&utm_source=google&utm_campaign=20059871949&utm_adgroup=148461901613&utm_content=ebook&utm_offer=understanding-etl&utm_ad=679932278073&utm_term=databricks etl&gad_source=1&gclid=Cj0KCQjw0Oq2BhCCARIsAA5hubXcrNmPj8L9jObjMLOvvnwctA7YqV99x9AgtNVN-f0jVMQzi5ekqu8aAunsEALw_wcB), Matt Palmer defines data ingestion as the process of **collecting and moving data ** from one or more **sources to a destination** system, such as a database, data warehouse, or data lake, where the data can be stored and processed for later analysis. In other words, it is the act of bringing data into an environment where it can be manipulated and utilized (Palmer, 2024).

 

<h4>DATA LAKE : DATABRICKS FILE SYSTEM</h4>

First, in the Data Lake on *Databricks*, we create the structure below:

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd5OZq27itnekDUzHYU7xx1Tzf0BZzgwhWNwxFBBJ8DLVRsO0PnjRW1R2pCchtMZTqO5t6l0kR3CyLomvxw9FtYnerT_FpZ4O82fQcqaWyiE85DWBl7rlCwX9C5KHRK4Oi7KWTLVCpGtuR-Ot1e4yUN_b51?key=PWW0ZQYwDDUibpv7ZqBUIQ)

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdodSU2u92p9Edytt1Ork3TtoLfMXmAeJKnA8ZyClzppGwqxXvz5USSR8kVXMcIofpVQvOS4LC7UX3mJY2CYk44RUE-PqWbAJsD8OgSTyCwVFJaVMMRg97ShdTkh0aFY84KyjcPmXhuCdXqMm84OL2MyFot?key=PWW0ZQYwDDUibpv7ZqBUIQ)

Upload to the Data Lake, locate the file, and load it:

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfPyXMSNQzGNGkthui6_5X2WOHsPfbXNT5odI6HOgkLuhsfDDFUebCLrE2lBxD1R71uOn9FaJT9DCoPYjFue5mW_0pC_Tpc8DcezdWiNx3wtOIq9GUDzHuUxrUNZwXuuFge-rRG6kVPKOGRaRxCQZeBlS4?key=PWW0ZQYwDDUibpv7ZqBUIQ)

We can see it:

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfoBsHQBTSeG3MmLMYAUBk0ogg8NHMGdF_wb1xuA8pBDjfdYRaQSr15ZmxRgCDbLheSzv6z8QuyNiy3Fi1PEh6Ii0PhfSBq2I3xluPNslXDyalZCYf9QlUH91yV4TyjFarixPYhQTQ0WU8eV_wqEh1V7yA?key=PWW0ZQYwDDUibpv7ZqBUIQ)



<h4> BRONZE LAYER</h4>

Save the file in Delta format in the Bronze layer:

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXexrMmlzRSannGvOhr9ZreCuCYZCj9lXHXMv4gdotRvoVoms3kMZyYIbrz4U_zaWzP9hBbpJIUePjH3a39RHmC5TYHwfJKi-VsMsNoWXB7wjdQzrOciSp8ZFGdoTIe4w-ZjQ9InomzbIFEg-JdbEVv4YDvk?key=PWW0ZQYwDDUibpv7ZqBUIQ)

This layer storage the raw data.

<h4> SILVER LAYER </h4>

**Flow:** Load from the Bronze layer, transform, and save to the Silver layer.

Starting from the raw data in the Bronze layer, we perform preprocessing and the necessary transformations. After the transformations, we save the data to the **Silver layer.**

- Renamed Columns: `withColumn`
- Replace \n : `regexp_replace` a `sql.function`
- Convert type column `price` to a `floatype`
- Filter()
- Count()



**Renamed Columns**

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc6eA5jhNe7c2rCHSSWc_j-WEw912-SNWTx6Uqj8_zkwkTPdWIpdjw99F9Q9QWWmNy2hC-JBYgnxNxF_SWy3cIQDgtaJC0Ap3N6o4bvo8TcQSEU0k-AAAugymDl-OkWB8ziraUiUSO5PRFL2EZvYQ78_fA?key=PWW0ZQYwDDUibpv7ZqBUIQ)



Note that the file in the bronze layer remains unchanged; it is still raw.

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdRpTTIsznylwMq2wLPIUgITi_mcLKnmZ0KGTpxrn2rYRDkI9cEalxrG0mnWdP-MOgN6Hy1SeUN4Z8lzTDduG5DTEF5fcalI8zwmGy45oF1PcSWmtIvL6cHSDPcNy9V_TYITZSfpCXE4SgJLjR1jz6jKz2s?key=PWW0ZQYwDDUibpv7ZqBUIQ)



:bulb: ​**Lazy Evaluation**

The bronze layer remains intact because no action has been performed on it.

This happens because when we load data from the bronze layer, we are only creating a DataFrame in memory. When we apply transformations (such as `filter`, `select`, etc.), Spark creates an **execution plan but doesn't execute** the operations yet.

In order for the transformations to be actually executed and the results saved in the silver layer, we need to call **an action**, such as `write`, `count`, `show`, etc. Only after this call will Spark execute the execution plan and process the data.

This is an example of Lazy Evaluation!



**Replace :** `\n`, `km |`, `R$`  in **Year, mileage and price columns**

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcOsFJmXG-qDhNfGa7IeZH3MGLfH1wyX8KyortfdK8fzZ2pq8rkg5LOGxfLIl9gMXP085Mart42yB6vdOU_0X-AFrUA65QsMMBmCXwXbKKepgXdi4EqTnOYgz5X1LN7AtY2VaWgqp7LKl2JBEeIxfc1MfIM?key=PWW0ZQYwDDUibpv7ZqBUIQ)

So, we will use `regexp_replace` to replace it with an empty string.

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfAAwwXFRZS4F1ris_5NZ9lh8vQFZvM0G3h2rcYKDs5fssiHOecMiY1tTrLrOlv0Jutwd95bPsW23TcqovyEXbk7b2ynT8vf7vb87CTwldSstORxTAZk1rTYiY7TpBtH25z6kjqmvubQnH6qZ6DvdlW5hJ8?key=PWW0ZQYwDDUibpv7ZqBUIQ)

**Convert Data Type in 'price' and 'mileage' columns:**

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcqxD2G6kibb_yu1Ozid59mObFt16zNRYb4oYAniEmdmScz57FwLUD2gz-bltvt9MuN_EK5engYc6WdTIrEVaVUNtO8trztkX_T5h5Fixh53_qfqaOtdgW3ttbq8JLmZtBZXmb0umwZVAThqE-DNWkUcS8?key=PWW0ZQYwDDUibpv7ZqBUIQ)

To standardize the "year" column: Extract the first year from the 'year' column and convert to integer.

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXccUsDyff8xLXHUiE1nNj8ykL1sPHDjbFwLpNGEhlXqP3baRTE1eofDJrvnGz6Rvt_2xVIa7ZebFv1xZgPHUUJzD_t9N3uAFf6--e7by8fhIeRWA14zpogw3Qv-e5Q0oQQT0eS3E0ARiS7wRLeBLmcSoXw?key=PWW0ZQYwDDUibpv7ZqBUIQ)

**`regexp_extract("year", r'(\d{4})', 1)`**: This extracts the first four-digit year from the `year` column.

**Create a new column `age`:**

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcV5qK1rjSJhLfEJyt8UlXO_lMTA70qJ0wHKZV7VJpXdU8HlV9YoB-v08iX7XvzKY2IzZJeESWJB3CW849wfLrlHFK23_4Ond6zAu7KZiMr9rMcW4FP-ZcgXhq71c0xyC5Hr2PN3HJBeZDtuo_nQhYxjzQq?key=PWW0ZQYwDDUibpv7ZqBUIQ)

**Filter columns**

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfpoZnwisN3ucNIzM856OQgcq94Fu-aUS_9iQN9t7JoAy3Tew0cUq8_56as2S-HiyG-WO_sGByMCGf84UZ2nCS10I0uGnARBnM01wryKpwh4gFbiGyZYryo5EvIwT3oFhPu0vIPgGffyUCxOjpIKBPcP_Q?key=PWW0ZQYwDDUibpv7ZqBUIQ)

**Filter columns with WHERE**

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdneVAjp7Vjo5QklQNxNUjjkaZkj7lKjaDUYyT1hUewxhqGAvYLfCjV01rePA0mnLPkxwr-Gk3DF2kKB-LitiOJfWDH2XiioPlBOT4eP3YIgLvZB3fB-FTvRoKhRpDRoC41j4Fi5Tdfu_bnNshRoCHBO_A?key=PWW0ZQYwDDUibpv7ZqBUIQ)



**OrderBy**

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXddwPf7JiZ8SY61912ztBdCRTsLj5CDOCQR7qBdeSFaXRAt2c8ZYs9_sQbnhRNrGWXHvrAidm6dG9rrA_ZKvuh4CwZlGQgPXP2uffnQnqqYWxEZfd-y4JPUCpy0pH9J25Jil1PVnYO_nZEu4H8VqW0F2KZL?key=PWW0ZQYwDDUibpv7ZqBUIQ)



**Count() specific rows**

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfgYr3yONHVFobIfWsuggDaCByKGh29q4hAO8qBJF_2EFLBRbAMoSelGu7KzlFBZavOsM89__lGwaarOiK-jq0jfJqZKj_NAo4Di-VoDB43WT1Yv4AqK-yY8Whr-iPRxOngC8kX25cIEip5zP7cxoPJc40?key=PWW0ZQYwDDUibpv7ZqBUIQ)





**Save SILVER LAYER**



![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXddx-CpdJqaF4lcdvU_VSwXWpC5jCloSsCu4yl09k1aFOcSEcIH-gP0Y3UpfOmXjmGYexW1hNwZNlkd938I92rG349E6PW54MbqF7VY0xiL7LLYsdEOxkggArQ2P0nTIZXFFS9Bs1AshVKzgJQHJueic6zo?key=PWW0ZQYwDDUibpv7ZqBUIQ)



**GOLD LAYER**



![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf3E1TrHL0LmsRgavMikd0ScNKIsHhQ--wIYdiBhdozDzTJMuetYiu5GWLdmGhRtTXDwZybNg3h6Yo0yYz7ynZE59Z2j89vjIK7tT9LnKFBK-4z6gyJtbHHNABTBfQhwGg0TBIaxxHQsjkChEezUu-xZ8Y?key=PWW0ZQYwDDUibpv7ZqBUIQ)

Checking the Gold layer

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcgBgbJkxe0HSw71zz22QWNxZuNhgiCLTqjDEVm-2IStMVXym43v4uwk83zazpJHgH6c2mZ094yYie_lp5FSXgvZSAtfVivCXBzyI9QBM_1nLaAPdk6uqUzu71BYaWjlMVt5jbAKwpB4Jivk2MGajB9tOWV?key=PWW0ZQYwDDUibpv7ZqBUIQ)

The directory contains two components: `_delta_log` and the **Parquet file**.

**Delta Log**:

- **What it is**: It is a directory called `_delta_log` that contains JSON files. These files log the transactions and operations performed on the Delta table, allowing for version management and data recovery.
- **Function**: The Delta Log provides version control and ensures the atomicity of operations. It enables operations like "time travel" (accessing previous versions of the data) and facilitates recovery in case of failures.

**Parquet File**:

- **What it is**: It is the actual file that contains the data stored in Parquet format. Parquet is a columnar storage format optimized for querying.
- **Function**: It stores the table data in an efficient format, allowing for fast queries and saving space.

:exclamation: When we read the file, we read it in **Delta format**, **not in Parquet format**.



**Comunity Databricks**

**Catalog**:

- **`spark_catalog`**: In the Community version, we generally only have access to the default catalog called `spark_catalog`. This means we cannot create custom catalogs, but we can use the default catalog to manage data.
- **Schemas and Tables**: Within the default catalog, we can create schemas and tables to organize the data.



![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeLJbPD8v1XDiNmuoRykSKrYiFCaxbvwY127hup0t1VKprQxRsmTZTsYG7ToFvIwMrZgH39XSBuIe5odxiljdVG2dB751wmmlWOZisIERADf3UttHhJpOkCjnWiW7xCEeFriiDz9fGA_T7n4fRol_Gj1dw?key=PWW0ZQYwDDUibpv7ZqBUIQ)

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfl0M2lfcKQwEpNCnsL9Hs3OGei7vi9GdN9H6aooxd56MSVZUdjKpMg9uFDNvGvY8Rk1nYyqrXh1yGcAqx6NGMsp5O2CuyM8UjImJD_AByABJpXMOQ7vxUs3Wm7v-8rr2JwSDWHAB2iz27bWxhQf1nMkCko?key=PWW0ZQYwDDUibpv7ZqBUIQ)





:bulb: **Pandas and Spark**

In the chapter "What Is Data Transformation?" from the book *[Understanding ETL](https://www.databricks.com/resources/ebook/understanding-etl?scid=7018Y000001Fi0cQAC&utm_medium=paid+search&utm_source=google&utm_campaign=20059871949&utm_adgroup=148461901613&utm_content=ebook&utm_offer=understanding-etl&utm_ad=679932278073&utm_term=databricks etl&gad_source=1&gclid=Cj0KCQjw0Oq2BhCCARIsAA5hubXcrNmPj8L9jObjMLOvvnwctA7YqV99x9AgtNVN-f0jVMQzi5ekqu8aAunsEALw_wcB)* by Matt Palmer(2024), theres a list of Python libraries and frameworks for data transformation. This chapter was important as it delved deeper into Pandas and Spark.

While Pandas excels in analyzing and transforming datasets that fit into memory, Spark is more suitable for large-scale data processing and distributed computing. Depending on the scale and resources needed, you might choose one tool or combine both for optimal results.

The history of Apache Spark is closely tied to the need for an alternative to Hadoop's MapReduce, as it was developed at the University of California, Berkeley, in response to MapReduce's limitations. Spark's key innovation is the Resilient Distributed Dataset (RDD), which enables in-memory data processing and faster computations.

Apache Spark also has a strong connection with Databricks, a company founded by its original creators, which offers a unified analytics platform for data engineers, scientists, and analysts. Databricks simplifies the adoption of Spark and enhances collaboration, making it a key player in modern data analytics.





**REFERENCES:**

:pushpin:[*Understanding ETL - Matt Palmer (2024)*](https://www.databricks.com/resources/ebook/understanding-etl?scid=7018Y000001Fi0cQAC&utm_medium=paid+search&utm_source=google&utm_campaign=20059871949&utm_adgroup=148461901613&utm_content=ebook&utm_offer=understanding-etl&utm_ad=679932278073&utm_term=databricks etl&gad_source=1&gclid=Cj0KCQjw0Oq2BhCCARIsAA5hubXcrNmPj8L9jObjMLOvvnwctA7YqV99x9AgtNVN-f0jVMQzi5ekqu8aAunsEALw_wcB)

:pushpin:[Learning Spark The definitive guide - Matei Zaharia](https://analyticsdata24.wordpress.com/wp-content/uploads/2020/02/spark-the-definitive-guide40www.bigdatabugs.com_.pdf)

:pushpin: [Get-Start-with-Spark-Dataframe - Databricks](https://www.databricks.com/spark/getting-started-with-apache-spark/dataframes)
