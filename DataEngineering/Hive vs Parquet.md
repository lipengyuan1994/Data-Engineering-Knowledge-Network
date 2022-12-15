







---

# [Spark Dataset on Hive vs Parquet file](https://stackoverflow.com/questions/51190646/spark-dataset-on-hive-vs-parquet-file)


[Ask Question](https://stackoverflow.com/questions/ask)

Asked 4 years, 5 months ago

Modified [4 years, 5 months ago](https://stackoverflow.com/questions/51190646/spark-dataset-on-hive-vs-parquet-file?lastactivity)

Viewed 3k times

4

[](https://stackoverflow.com/posts/51190646/timeline)

I have 2 instances for the same data.

1.  [[Hive]] table called myData in parquet format
2.  [[Parquet]] file (not managed by Hive) that is in parquet format

Consider the following code:

    val myCoolDataSet = spark
        .sql("select * from myData")
        .select("col1", "col2")
        .as[MyDataSet]
        .filter(x => x.col1 == "Dummy")
    

And this one:

    val myCoolDataSet = spark
        .read
        .parquet("path_to_file")
        .select("col1", "col2")
        .as[MyDataSet]
        .filter(x => x.col1 == "Dummy")
    

My question is what is better in terms of performance and amount of scanned data? How spark computes it for the 2 different approaches?

*   [scala](https://stackoverflow.com/questions/tagged/scala)
*   [apache-spark](https://stackoverflow.com/questions/tagged/apache-spark)
*   [parquet](https://stackoverflow.com/questions/tagged/parquet)

[Share](https://stackoverflow.com/q/51190646)

[Improve this question](https://stackoverflow.com/posts/51190646/edit)

Follow

asked Jul 5, 2018 at 11:50








[](https://stackoverflow.com/posts/51190730/timeline)

Hive serves as a storage for metadata about the Parquet file. Spark can leverage the information contained therein to perform interesting optimizations. Since the backing storage is the same you'll probably not see much difference, but the optimizations based on the metadata in Hive _can_ give an edge.