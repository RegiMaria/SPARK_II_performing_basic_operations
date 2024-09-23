<h3>SPARK Escape Character:</h3>

-------------

**CSV**

**CSV** stands for Comma-Separated Values. It is a common text file format where each line represents a single record, and commas separate each field within a record.

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfgEQertV8pNpIV6-B1q7VxRQ1cLn5zXidMZQrTlyMFCpByoAhvSjUvkm47T2B1pqHDuOWBNuqTX7B-tFf9uyGTw4RQlvGm5JxBHL7rCptGPe8si2UXXzDvTJjTnlvJl9MQodT27ucEyih4zcr_dxzVUsdN?key=PWW0ZQYwDDUibpv7ZqBUIQ)

Although CSV files may seem well-structured, they are actually one of the most complicated file formats you will encounter, because in production scenarios, you can't make many assumptions about what they contain or how they are structured. CSV files are fragile!



### Reading CSV in Spark:



![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdIBVbMUn-9syfd5sYtVYI6aPQEM_nvXa67yfmizFkR0T4-jzwxDO0u1OX2ui62_5UAwTVwNvsCpiclje1cb3F41UTcoLiOzB3xjHvm-jgEHfyjzDwuiYevSnWVHYTs2E66eZ4LGV1AF6TxqVO1We1lUEZs?key=PWW0ZQYwDDUibpv7ZqBUIQ)

​			



**Reserved Characters and Keywords**

One issue you may encounter is reserved characters, such as spaces or dashes, in column names. Dealing with these characters means properly escaping the column names. Most of the time, we try to resolve this issue with character escaping during data preprocessing or transformation, but Spark's documentation offers a simple solution for handling reading, writing, and loading CSV data.

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeWTzUs55K5q5DbqFv4SAU_Y1mbvOlEIwLmJtSrk-AbECAkhuCMMEG6XcgOmRMsFm14wMTZJd06VcxECTJiPNFlsVDPhuB1OVd2GHUx0_es8LXKs_JVQpjLYd99D24Zd-F8ZRFhR7LMr4eZaM1IqwGEicxo?key=PWW0ZQYwDDUibpv7ZqBUIQ)

​	**Book Reference**: *Learning Spark - Chapter 6 - Data Source / CSV Files - Page 165*



Written by Matei Zaharia himself (the developer of Spark), the reading of CSV in Spark is extensively addressed.

While we still face issues with special characters and line breaks within columns, the major problems in reading CSV files have been resolved.



![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfQPD1Yi7TWpCHuqyqZhLUdfq6evQqxO6nv-KKHbEJLANN6uzyXVKJPbwjZlSG2rSCTA4zTjP0F1hXFPIujlU-5BdTze_Gki3yhiH6kYRk_WqU7LJJxgQU_8y7gzwy_QycqeyHdaN1u8DOZ1-yiYrnEUf4?key=PWW0ZQYwDDUibpv7ZqBUIQ)

Reading the documentation can save you hours of research on how to solve these issues. Make use of it.



Dealing with a document like this would involve steps such as specifying an ID, moving data from later rows to earlier rows, removing duplicates, and handling broken lines. While it is possible to do, it would take more time to correct the data and prepare it for preprocessing before moving on to the transformation stage.



![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcfes7jkvY4QzBHKx5wQEScda83w5bpsUrnv0Io3y_WFTld53ctdQdI-HKfm_tUMV6jVDfRMLt1sbPRKTAU0QTTVC3JYk0wkTi26EnmlCHboozu0N-uh5eRYUn7ShpK6Dj129xE8v5-5-HIX6xNB9ALQPtN?key=PWW0ZQYwDDUibpv7ZqBUIQ)





:pushpin:[:Spark-Definitve-guide-Matei-Zaharia](https://github.com/VolodymyrGavrysh/My_RoadMap_Data_Science/blob/master/kds/books/Spark-The%20Definitive%20Guide.pdf)

