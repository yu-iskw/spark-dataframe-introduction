# Set up Spark and Data Sets on Your Local Machine

## Download the Data Sets

You can prepare for the data sets by the bash script.
In order to execute the script, you should install mongoDB to use `bsondump` which converts a BSON format to JSON format.
As a result of executing the script, the data sets are downloaded in `/tmp/github-archive-data`.

```
bash src/bash/prepare-data-local.sh

cd /tmp/github-archive-data
ls -l
2015-01-01-0.json.gz   2015-01-01-17.json.gz  2015-01-01-4.json.gz
2015-01-01-1.json.gz   2015-01-01-18.json.gz  2015-01-01-5.json.gz
2015-01-01-10.json.gz  2015-01-01-19.json.gz  2015-01-01-6.json.gz
2015-01-01-11.json.gz  2015-01-01-2.json.gz   2015-01-01-7.json.gz
2015-01-01-12.json.gz  2015-01-01-20.json.gz  2015-01-01-8.json.gz
2015-01-01-13.json.gz  2015-01-01-21.json.gz  2015-01-01-9.json.gz
2015-01-01-14.json.gz  2015-01-01-22.json.gz  dump
2015-01-01-15.json.gz  2015-01-01-23.json.gz  github-users.json
2015-01-01-16.json.gz  2015-01-01-3.json.gz
```

## Build Spark on Your Local Machine

You should build Apache Spark on your local machine to use it.
It takes a long time to compile Spark with `sbt` for the first time.
So I recommend you to have a break for compiling.

This documentation is based on Spark 1.3 RC1, because Spark 1.3 have not released at Feb, 25 2015 yet.

```
git checkout https://github.com/apache/spark.git && cd spark
git checkout -b v1.3.0-rc1 origin/v1.3.0-rc1
./sbt/sbt clean assembly
```
