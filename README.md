# tasks
The scripts were implemented on GCP Ubuntu 18.04.
Steps to be followed:-

1. Run the script hdp.yml to install hdp 2.8.5 standalone mode
2. Run the script click.yml to install clickhouse
3. Loaded the data(rec.csv) from hdfs to Clickhouse. Please refer op1.png.

Please note that the above scripts are not reproducible. You need to remove the packages before running the script again or it may fail. 

I ran out of my free GCP VM and was not able to test transforming the csv from hdfs to clickhouse. But I tried below to give the context on how it may be achieved:-

=======================================
from pyspark.sql import sqlcontext
from pyspark.sql import io.clickhouse.spark.connector._
from pyspark.sql.types import *
>>>  df = sqlContext.read.load('file:///path.csv', 
                          format='com.databricks.spark.csv', 
                          header='true', 
                          inferSchema='true')
>>> df.count()
>>> df.dtypes
>>> df.describe.show() 
df=sqlContext.read.format("jdbc").options(Map("url" -> "jdbc:clickhouse://localhost:8123", "dbtable" -> Query, "driver" -> "ru.yandex.clickhouse.ClickHouseDriver")).load()

=====================================================

Before running the above spark code we need to :-
1. Copy spark-clickhouse-connector_2.11-2.4.0_0.21.jar to spark lib directory
2. Copy ru.yandex.clickhouse.clickhouse-jdbc to spark lib directory
3. Add dependency to your spark project
        <dependency>
            <groupId>io.clickhouse</groupId>
            <artifactId>spark-clickhouse-connector_2.11</artifactId>
            <version>0.21</version>
        </dependency>
