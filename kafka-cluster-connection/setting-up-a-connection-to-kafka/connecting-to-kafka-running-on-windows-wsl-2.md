# Connecting to Kafka running on Windows WSL 2

## Apache Kafka on Windows

Thanks to WSL 2, Apache Kafka can run properly on Windows. You can read a how-to guide at Confluent: [https://www.confluent.io/blog/set-up-and-run-kafka-on-windows-linux-wsl-2](https://www.confluent.io/blog/set-up-and-run-kafka-on-windows-linux-wsl-2/#start-kafka-cluster)

## Conduktor runs on your host network

If you're running your Apache Kafka brokers using WSL2 and want to connect Conduktor to your brokers, you may run into some errors due to misconfigurations. For instance, if you try to connect to "localhost:9092" or "127.0.0.1:9092" and this does not work, please continue reading. 

It's similar to using Docker: Conduktor is running on your host system, on your host network. It is running outside of your internal Docker network, and here, it is running outside of your WSL 2 network. You must "connect" both.

## WSL 2 is not "done" yet

WSL 2 has a few networking issues, which can cause these connectivity issues. Please check for answers:

* [https://stackoverflow.com/questions/64177422/unable-to-produce-to-kafka-topic-that-is-running-on-wsl-2-from-windows/65553634](https://stackoverflow.com/questions/64177422/unable-to-produce-to-kafka-topic-that-is-running-on-wsl-2-from-windows/65553634#65553634)
* [https://github.com/microsoft/WSL/issues/4851](https://github.com/microsoft/WSL/issues/4851)

## Two ways to fix this

### IPv6

* Use IPv6 loopback address in your broker \(server.properties\)

```text
listeners=PLAINTEXT://[::1]:9092
```

* Same in Conduktor, for your bootstrap address:

```text
[::1]:9092
```

### 172.x

Find the address of your WSL 2 network:

```text
$ ip addr | grep "eth0"
172.x.y.z
```

* Use this in your broker \(server.properties\)

```text
listeners=PLAINTEXT://172.x.y.z:9092
```

* Same in Conduktor, for your bootstrap address:

```text
172.x.y.z:9092
```

