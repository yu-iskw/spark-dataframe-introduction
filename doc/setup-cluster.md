# Set Up Spark Cluster on EC2 and Data Sets

## Check Out Apache Spark from Github

At first, please check out spark from Github on your local machine.

```
git checkout https://github.com/apache/spark.git && cd spark
git checkout -b v1.3.0-rc1 origin/v1.3.0-rc1
```

## Launch a Spark Cluster ver. 1.3 on EC2

`$SPARK_HOME/ec2/spark-ec2` command allow you to launch Spark Clusters on Amazon EC2.
Where, the checked out directory is defined as `$SPARK_HOME`.

You can launch a Spark Cluster with the below commands in Tokyo region on Amazon EC2.
So, you should change the region for your location.

```
cd $SPARK_HOME

REGION='ap-northeast-1'
ZONE='ap-northeast-1b'
VERSION='1.3.0'
MASTER_INSTANCE_TYPE='r3.large'
SLAVE_INSTANCE_TYPE='r3.8xlarge'
NUM_SLAVES=5
SPORT_PRICE=1.0
SPARK_CLUSTER_NAME="spark-cluster-v${VERSION}-${SLAVE_INSTANCE_TYPE}x${NUM_SLAVES}"

./ec2/spark-ec2 -k ${YOUR_KEY_NAME} -i ${YOUR_KEY} -s $NUM_SLAVES --master-instance-type="$MASTER_INSTANCE_TYPE" --instance-type="$SLAVE_INSTANCE_TYPE" --region="$REGION" --zone="$ZONE" --spot-price=$SPOT_PRICE --spark-version="${VERSION}" --hadoop-major-version=2 launch "$SPARK_CLUSTER_NAME"
```

## Download Data Sets on EC2

Please log in with ssh to the master instance which was created by `$SPARK_HOME/ec2/spark-ec2`.
And then execute the shell script to download the data sets for this introduction.
Off cource, You should check out this introduction from Github on the instance.

```
bash ./src/bash/download-for-cluster.md
```
