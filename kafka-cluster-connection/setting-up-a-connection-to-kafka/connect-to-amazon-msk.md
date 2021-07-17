# Connect to Amazon MSK

## What is MSK?

Amazon MSK is a self-managed service that makes it easy to build and run applications that use Apache Kafka to process streaming data. 

It lacks several important Apache Kafka features like Kafka Connect, Kafka Streams, ksqlDB, and is not cloud-native \(serverless, like S3 or Kinesis\) but is just a provisioned infrastructure

## Conduktor & MSK

Conduktor, which is running on your computer, has no access by default to MSK. Still, it's possible to connect it to the cluster by using a specialized kafka proxy in-between.

To make it work:

* Start a proxy [https://github.com/dajudge/kafkaproxy/](https://github.com/dajudge/kafkaproxy/) on a EC2 instance that has access to the cluster. For instance, using Docker:

```text
$ sudo docker run --net host \
    -e KAFKAPROXY_HOSTNAME=localhost\
    -e KAFKAPROXY_BASE_PORT=4000 \
    -e KAFKAPROXY_BOOTSTRAP_SERVERS=MYBROKER1:9092,broker2:9092,broker3:9092 \
    dajudge/kafkaproxy:0.0.13

```

* On your local machine, do a ssh-tunnel to this EC2 instance:

```text
$ ssh -i ~/.ssh/ec2-key.pem -N \
     -L 4000:localhost:4000 \
     -L 4001:localhost:4001 \
     -L 4002:localhost:4002 \
     <ec2instance>
```

* Connect Conduktor using localhost:4000

## Connect using AWS IAM

Conduktor fully handles AWS IAM, you just have to setup your connection with your IAM access.

Read our guest blog on AWS for more details: [https://aws.amazon.com/blogs/big-data/securing-apache-kafka-is-easy-and-familiar-with-iam-access-control-for-amazon-msk/](https://aws.amazon.com/blogs/big-data/securing-apache-kafka-is-easy-and-familiar-with-iam-access-control-for-amazon-msk/)

![](../../.gitbook/assets/bdb1447-access-control-msk-4.gif)

### AWS MSK + IAM Architecture

A small overview of "what's going on" when you use AWS MSK and configure IAM \(read the mentioned blog above for more details\):



![](../../.gitbook/assets/image%20%2846%29.png)

### Configuration Example

Here is an example of configuration you can copy/paste. Just update the `awsProfileName` to yours:

```text
security.protocol=SASL_SSL
sasl.mechanism=AWS_MSK_IAM
sasl.jaas.config = software.amazon.msk.auth.iam.IAMLoginModule required awsProfileName="stephane-msk";
sasl.client.callback.handler.class=software.amazon.msk.auth.iam.IAMClientCallbackHandler 
```

