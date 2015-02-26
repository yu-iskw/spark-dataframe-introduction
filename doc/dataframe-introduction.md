# DataFrames Introduction

## Create DataFrames

You should arrange the downloaded JSON data before using Spark DataFrames.
And You can also create dataframes from Hive, Parquet and RDD.

You know, you can run a Spark shell by executing `$SPARK_HOME/bin/spark-shell`.

If you want to treat JSON files, use `SQLContext.load`.
And I recommend you to set their aliases at the same time.

### On Local Machine

```
// Create a SQLContext (sc is an existing SparkContext)
val context = new org.apache.spark.sql.SQLContext(sc)

// Create a DataFrame for Github events
var path = "file:///tmp/github-archive-data/*.json.gz"
val event = context.load(path, "json").as('event)

// Create a DataFrame for Github users
path = "file:///tmp/github-archive-data/github-users.json"
val user = context.load(path, "json").as('user)
```

You can show a schema by `printSchema`.

```
event.printSchema
user.printSchema
```

### On Spark Cluster

```
// Create a SQLContext (sc is an existing SparkContext)
val context = new org.apache.spark.sql.SQLContext(sc)

// Create a DataFrame for Github events
var path = "file:///mnt/github-archive-data/*.json.gz"
val event = context.load(path, "json").as('event)

// Create a DataFrame for Github users
path = "file:///mnt/github-archive-data/github-users.json"
val user = context.load(path, "json").as('user)
```

## Project (i.e. selecting some fields of a DataFrame)

You can select a column by `dataframe("key")` or `dataframe.select("key")`.
If you have select multiple columns, use `data.frame.select("key1", "key2")`.

```
// Select a clumn
event("public").limit(2).show()

// Use an alias for a column
event.select('public as 'PUBLIC).limit(2).show()

// Select multile columns with aliases
event.select('public as 'PUBLIC, 'id as 'ID).limit(2).show()

// Select nested columns with aliases
event.select($"payload.size" as 'size, $"actor.id" as 'actor_id).limit(10).show()
```

## Filter

You can filter the data with `filter()`.

```
// Filter by a condition
user.filter("name is null").select('id, 'name).limit(5).show()
user.filter("name is not null").select('id, 'name).limit(5).show()

// Filter by a comblination of two conditions
// These two expression are same
event.filter("public = true and type = 'ForkEvent'").count
event.filter("public = true").filter("type = 'ForkEvent'").count
```

## Aggregation

```
val countByType = event.groupBy("type").count()
countByType.limit(5).show()

val countByIdAndType = event.groupBy("id", "type").count()
countByIdAndType.limit(10).foreach(println)
```

## Join

You can join two data sets with `join`.
And `where` allows you to set conditions to join them.

```
// Self join
val x = user.as('x)
val y = user.as('y)
val join = x.join(y).where($"x.id" === $"y.id")
join.select($"x.id", $"y.id").limit(10).show

// Join Pull Request event data with user data
val pr = event.filter('type === "PullRequestEvent").as('pr)
val join = pr.join(user).where($"pr.payload.pull_request.user.id" === $"user.id")
join.select($"pr.type", $"user.name", $"pr.created_at").limit(5).show
```

## UDFs

You can define UDFs (User Define Functions) with `udf()`.

```
// Define User Defined Functions
val toDate = udf((createdAt: String) => createdAt.substring(0, 10))
val toTime = udf((createdAt: String) => createdAt.substring(11, 19))

// Use the UDFs in select()
event.select(toDate('created_at) as 'date, toTime('created_at) as 'time).limit(5).show
```

## Execute Spark SQL

You can manipurate data with not only DataFrame but also Spark SQL.

```
// Register a temporary table for the schema
event.registerTempTable("event")

// Execute a Spark SQL
context.sql("SELECT created_at, repo.name AS `repo.name`, actor.id, type FROM event WHERE type = 'PullRequestEvent'").limit(5).show()
```
