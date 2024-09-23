<h3>SPARK ESTRUCTURED APIs : DATAFRAME - PERFORMING BASIC OPERATIONS </h3>



------------------------------

**OBJETIVO: **Performing operations on SPARK API DataFrames in Databricks platform.

Before starting the preprocessing and transformation stage, there are a few things you need to know about Spark. In this project, we will study about:

:pushpin: Overview Structured SPARK APIs 

:pushpin:Spark SPARK Escape Character

:pushpin:Medallion Architecture

:pushpin:**​Narrow Transformations** and **Wide Transformations** in DataFrames

:pushpin:**Task, Stage and Job** in Apache Spark.



**WORKFLOW**

------------------------------



<table>   <thead>     <tr>       <th>Step</th>       <th>Description</th>     </tr>   </thead>   <tbody>     <tr>       <td>1. Load sample data</td>       <td>Import raw data from a CSV file into the landing layer (Data Lake/DBFS).</td>     </tr>     <tr>       <td>2. Transform Data</td>       <td>Clean, validate, and enrich the data using the Spark API. This step includes data preprocessing and transformation to prepare for analysis.</td>     </tr>     <tr>       <td>3. Save Transformed Data</td>       <td>Store the transformed data in a Delta Table or another optimized storage format for further analysis.</td>     </tr>     <tr>       </table>



**ARCHITECTURE **

----------------------------------

In this project we use the [Medallion architecture](https://www.databricks.com/sites/default/files/2023-10/oreilly-delta-lake_-up-and-running.pdf). The Medallion Architecture organizes data into layers to ensure that data evolves in a clean and consistent manner throughout the pipeline. It's composed of three main layers:

- **Bronze (Raw Data):** Raw data as it comes from the source, without any transformation or cleaning.
- **Silver (Cleaned Data):** Cleaned, normalized, and standardized data. Errors are corrected, and the schema is consistent.
- **Gold (Aggregated Data):** Data ready for analysis, enriched and aggregated for reporting and business use.

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfNs0gzYfjQxyFg47PWeQXJ979fBq0Olmd-b1L2jf_5fbXKyhn_MqJMdbPPegXuZTpCjSicHp2hEKeLrWYGxxafFLaXVbxdITUfB5Ck6XzousRtit1XSY94GNnMVosWM_O7aA0ksfdnGN3VDkkQm_EtcOs?key=ospagYUfAVBDEUegIHMqiw)



**TECNOLOGIA**

-------------------------------------------

- **Data Lake **: DBFS (Databricks File System), which serves as the data lake for storing raw data.
- **Advanced Storage Technology**: Delta Lake, used to transform and manage data.



**Databricks Platform Components Used:**

- **Databricks Workspace**: The environment to development and execution of scripts and notebooks.
- **DBFS (Databricks File System)**: Distributed file system used as a data lake for storing data.
- **Delta Lake**: Data management technology that adds advanced functionalities to the data lake, such as ACID transactions and processing optimization.
- **Delta Tables**: Table format used to store and manage transformed data with Delta Lake.



**About the DataSet:**

**Dataset Size:**

- Number of rows: 516 rows.
- Number of columns: 6 columns.

**Columns and Data Types:**

- Column names: marca, modelo, ano, km,cambio and preco.
- Data types: All records are stored as strings.

**Formatting Issues:**

- Missing data.
- Inconsistencies:
  - Broken rows.
  - Values in inconsistent formats.
  - 'price' in inconsistent formats.
- Duplicate values: Identifying duplicate values.



**ABOUT PROJECT**

--------------------------------

This project will be done in two stages: Preprocessing and Transformation

**PREPROCESSING:** Preprocessing is a stage that enhances the **quality of the data**. Raw data is prepared, cleaned, and structured.

**TRANSFORMATION:** In the transformation stage, we manipulate the data. The primary goal is to prepar the data with specific needs of analysis or modeling.

Understanding these concepts helps to organize and structure the data workflow.

This project involves inspecting the dataset and building steps to fixed issues related to it.



**1. PREPROCESSING DATA**

-------------------------

**Examining data**:  it's essential that you know your data!  it's essential that you know your data!

:pushpin:Raw data here

The Dataset exhibits several issues or multiple problems, as we can see.

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeDjF7l2A3auwePTd2F-EL1d6P1rZmOp0Hcowp1m4GJgqYvPgoWWR-_pIjgTZHk3Ic6TFwOB8VhFWTt1was16ouiTPoG_VkWtQPfbfaBzf9GxIZUJFbld2fUHMhTAY57T0LaPArNCcOqkIDBlwPaeQ1tAtj?key=PWW0ZQYwDDUibpv7ZqBUIQ)

This happened because when reading the CSV,  **Spark needs to be specifically instructed on how to handle quotes, delimiters, and special characters**. So in this project, you'll learn the **correct way to read CSV files with Spark** to avoid this type of issue.

:rotating_light: The book *Spark: The Definitive Guide* by Matei Zaharia explains how to correctly read (writing and load) CSV files with Spark. For that you can see to the section on Spark's escape character.

:rotating_light: You find this in [How to get Start with Apache Spark on Databricks](https://www.databricks.com/spark/getting-started-with-apache-spark/dataframes) too.



**2. TRANSFORM DATA:**


-------------------------

Transformation might be as simple as removing unwanted records, filtering, or as complex as restructuring the source data entirely. 

**Data transformation** refers to additional manipulations that adjust the data's form for specific analyses, models, or reports(PALMER, pg. 25). 

Check lits:

- Schema

- Rename columns

- Filter

- Create derived column

- Convert data type



**ABOUT THE PIPELINE:**

----------------------------

**1. Bronze Layer (Raw Data)**

**Raw Data Loading**:

- Load the original CSV.
- Verify the schema.
- Store the raw data in a Delta Table in the "Bronze" format without any modifications.
- **Action:** `df_car_raw.write.format("delta").save("dbfs:/FileStore/cardealer/bronze")`

Obs: *The Bronze layer is crucial for **storing raw data** in its **original form**, without any modifications. The focus is on **preserving the integrity of the original data** for **auditing purposes** or for situations where the raw data needs to be **reprocessed**.*

-------------------------------



**2. Silver Layer (Data Cleaning and Transformation)** :star:

- **Cleaning and Preprocessing**:
  - Remove broken rows and incorrectly split columns (identify and correct <u>incomplete records</u>).
  - Correct column values that contain <u>unwanted characters</u>, such as "R$" and "km".
  - Convert data types, such as transforming monetary value columns from string to `FloatType`.
  - Format <u>date</u> and time fields.
  - Remove or correct duplicate or inconsistent values.
  - **Ensure that all fields have the correct data type and are properly formatted.**
- **String Manipulation**:
  - Clean columns like `km`, removing <u>unwanted strings</u> (" km").
  - Convert monetary values to floats.
- **Validation**:
  - Validate that all columns contain consistent and error-free values.
  - :point_right: Rebuild the DataFrame and verify the **final schema**. 
- **Action:** `df_car_silver.write.format("delta").save("dbfs:/FileStore/cardealer/silver")`

----------------------------------



**3. Gold Layer (Enriched Data)**

- **Enrichment and Aggregations**:
  - Calculate relevant metrics, such as average sales value, purchase value, or mileage.
  - Join external data, if necessary. (integrating with a historical price table).
  - Perform aggregations, filtering, and calculations to produce data ready for analysis.
- **Action**: `df_car_gold.write.format("delta").save("dbfs:/FileStore/cardealer/gold")`



In this project, we will also practice transformation operations with: 

**filter()**

**select()**

**OrderBy()**

**GroupBy()**

-------------------------------------------------

**4. Quality Check**

- **Data Quality Checks**:
  - Ensure that values in each field are within acceptable ranges (ex: onetary values should not be negative).
  - Verify that all transformations have been applied correctly.
- **Action**: Use logs and quality validation tools such as **Delta Lake time travel**.

--------------------------

**5. Save Final Data**

- Save the tables in Delta Lake to track changes in the data**(Time Travel)** and ensure data quality.
- Use **partitioning** to optimize queries and analyses.

--------------------------



:pushpin:How to do

:pushpin:Notebook

:pushpin:Key concepts

---------------------------



**TOOLS:**

:pushpin:[Databricks](https://community.cloud.databricks.com/login.html) (community edition)

:pushpin: Python

:pushpin: [Spark API](https://docs.databricks.com/pt/spark/index.html)

--------------------------------



**REFERÊNCIAS:**

:pushpin:Spark-definitive-guide-Matei-Zharia

:pushpin: [Databricks GitHub Spark](https://github.com/databricks/learning-spark)

:pushpin:[Learning Spark Lightning-Fast Big Data Analysis](https://github.com/VolodymyrGavrysh/My_RoadMap_Data_Science/blob/master/kds/books/Learning%20Spark%20%20Lightning-Fast%20Big%20Data%20Analysis%20.pdf)

:pushpin: [Github-Spark- the-definitive-guide](https://github.com/databricks/Spark-The-Definitive-Guide)

:pushpin:[DeltaLake Up and Running - Modern Data Lakehouse Architecture with delta lake](https://www.databricks.com/sites/default/files/2023-10/oreilly-delta-lake_-up-and-running.pdf)

:pushpin:[What is a medallion architecture?](https://www.databricks.com/glossary/medallion-architecture)

:pushpin:[Understanding ETL Data Pipelines for Modern Data Architecture - Matt Palmer](https://www.databricks.com/sites/default/files/2024-03/oreilly-technical-guide-understanding-etl.pdf)

:pushpin: [Guide to Apache Spark and delta lake](https://www.databricks.com/wp-content/uploads/2021/06/data-engineers-guide-apache-spark-delta-lake-v3.pdf)

[:pushpin:Spark Programming in Python for Beginners](https://www.oreilly.com/library/view/spark-programming-in/9781803246161/?_gl=1*1vi49oq*_ga*NTQ4OTUwNTc3LjE3MjQxODM2NjA.*_ga_092EL089CH*MTcyNDMzNTM2MS40LjEuMTcyNDMzNTcxMC4zOS4wLjA.)

