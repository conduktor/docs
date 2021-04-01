# Connect to Amazon MSK

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

  


