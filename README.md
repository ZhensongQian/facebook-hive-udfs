Facebook's Hive UDFs
==================


## WHAT IT IS

A computer guy at Facebook dumped a bunch of UDFs/UDAFs here:

https://issues.apache.org/jira/browse/HIVE-1545

However, the code does not build and is missing many parts.

This is a partial copy of that code, except it builds and may (or may not) work. Use at your own risk. To build it:

```
mvn package
```

This will produce a jar in `target/` which you can add to your Hive classpath.

Or simply download the `facebook-udfs-1.0.3-SNAPSHOT.jar`.

## HOW DO USE IT?

Like any other UDF, here's a sample:

```
CREATE TEMPORARY FUNCTION md5 AS 'com.facebook.hive.udf.UDFMD5';
SELECT md5(password) from users limit 1;
```
```
CREATE TEMPORARY FUNCTION myJaccard AS 'com.facebook.hive.udf.UDFJaccard';
select myJaccard(array('a','b','c'), array('b','c','d') ) as jaccard_similarity;
```

## Using with pyspark   
-- better to use this as the initial exploration step.

```
pyspark --jars facebook-udfs-1.0.3-SNAPSHOT.jar
```

```
sqlContext.sql("CREATE TEMPORARY FUNCTION myJaccard AS 'com.facebook.hive.udf.UDFJaccard'")
sqlContext.sql("SELECT  myJaccard(s1,s) as jaccard  FROM df2 ")


```
## Using with spark-submit 

```
spark-submit  --master yarn-client --num-executors 5 --driver-memory 10G --executor-memory 25G  --jars facebook-udfs-1.0.3-SNAPSHOT.jar  --packages com.databricks:spark-csv_2.10:1.4.0 --class com.databricks.spark.csv testUDF.py
```


