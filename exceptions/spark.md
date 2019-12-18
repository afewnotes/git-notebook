
#### Kafka offset 提交失败

* 异常详情，即在消费端提交前，当前会话已经超时
```
org.apache.kafka.clients.consumer.CommitFailedException: Commit cannot be completed since the group has already rebalanced and assigned the partitions to another member. This means that the time between subsequent calls to poll() was longer than the configured session.timeout.ms, which typically implies that the poll loop is spending too much time message processing. You can address this either by increasing the session timeout or by reducing the maximum size of batches returned in poll() with max.poll.records.
	at org.apache.kafka.clients.consumer.internals.ConsumerCoordinator$OffsetCommitResponseHandler.handle(ConsumerCoordinator.java:600)
	at org.apache.kafka.clients.consumer.internals.ConsumerCoordinator$OffsetCommitResponseHandler.handle(ConsumerCoordinator.java:541)
	at org.apache.kafka.clients.consumer.internals.AbstractCoordinator$CoordinatorResponseHandler.onSuccess(AbstractCoordinator.java:679)
	at org.apache.kafka.clients.consumer.internals.AbstractCoordinator$CoordinatorResponseHandler.onSuccess(AbstractCoordinator.java:658)
	at org.apache.kafka.clients.consumer.internals.RequestFuture$1.onSuccess(RequestFuture.java:167)
	at org.apache.kafka.clients.consumer.internals.RequestFuture.fireSuccess(RequestFuture.java:133)
	at org.apache.kafka.clients.consumer.internals.RequestFuture.complete(RequestFuture.java:107)
	at org.apache.kafka.clients.consumer.internals.ConsumerNetworkClient$RequestFutureCompletionHandler.onComplete(ConsumerNetworkClient.java:426)
	at org.apache.kafka.clients.NetworkClient.poll(NetworkClient.java:278)
	at org.apache.kafka.clients.consumer.internals.ConsumerNetworkClient.clientPoll(ConsumerNetworkClient.java:360)
	at org.apache.kafka.clients.consumer.internals.ConsumerNetworkClient.poll(ConsumerNetworkClient.java:224)
	at org.apache.kafka.clients.consumer.internals.ConsumerNetworkClient.poll(ConsumerNetworkClient.java:201)
	at org.apache.kafka.clients.consumer.KafkaConsumer.pollOnce(KafkaConsumer.java:999)
	at org.apache.kafka.clients.consumer.KafkaConsumer.poll(KafkaConsumer.java:938)
	at org.apache.spark.streaming.kafka010.DirectKafkaInputDStream.paranoidPoll(DirectKafkaInputDStream.scala:163)
	at org.apache.spark.streaming.kafka010.DirectKafkaInputDStream.latestOffsets(DirectKafkaInputDStream.scala:182)
	at org.apache.spark.streaming.kafka010.DirectKafkaInputDStream.compute(DirectKafkaInputDStream.scala:209)
	at org.apache.spark.streaming.dstream.DStream$$anonfun$getOrCompute$1$$anonfun$1$$anonfun$apply$7.apply(DStream.scala:342)
	at org.apache.spark.streaming.dstream.DStream$$anonfun$getOrCompute$1$$anonfun$1$$anonfun$apply$7.apply(DStream.scala:342)
	at scala.util.DynamicVariable.withValue(DynamicVariable.scala:58)
	at org.apache.spark.streaming.dstream.DStream$$anonfun$getOrCompute$1$$anonfun$1.apply(DStream.scala:341)
	at org.apache.spark.streaming.dstream.DStream$$anonfun$getOrCompute$1$$anonfun$1.apply(DStream.scala:341)
	at org.apache.spark.streaming.dstream.DStream.createRDDWithLocalProperties(DStream.scala:416)
	at org.apache.spark.streaming.dstream.DStream$$anonfun$getOrCompute$1.apply(DStream.scala:336)
	at org.apache.spark.streaming.dstream.DStream$$anonfun$getOrCompute$1.apply(DStream.scala:334)
	at scala.Option.orElse(Option.scala:289)
	at org.apache.spark.streaming.dstream.DStream.getOrCompute(DStream.scala:331)
	at org.apache.spark.streaming.dstream.ForEachDStream.generateJob(ForEachDStream.scala:48)
	at org.apache.spark.streaming.DStreamGraph$$anonfun$1.apply(DStreamGraph.scala:122)
	at org.apache.spark.streaming.DStreamGraph$$anonfun$1.apply(DStreamGraph.scala:121)
	at scala.collection.TraversableLike$$anonfun$flatMap$1.apply(TraversableLike.scala:241)
	at scala.collection.TraversableLike$$anonfun$flatMap$1.apply(TraversableLike.scala:241)
	at scala.collection.mutable.ResizableArray$class.foreach(ResizableArray.scala:59)
	at scala.collection.mutable.ArrayBuffer.foreach(ArrayBuffer.scala:48)
	at scala.collection.TraversableLike$class.flatMap(TraversableLike.scala:241)
	at scala.collection.AbstractTraversable.flatMap(Traversable.scala:104)
	at org.apache.spark.streaming.DStreamGraph.generateJobs(DStreamGraph.scala:121)
	at org.apache.spark.streaming.scheduler.JobGenerator$$anonfun$3.apply(JobGenerator.scala:249)
	at org.apache.spark.streaming.scheduler.JobGenerator$$anonfun$3.apply(JobGenerator.scala:247)
	at scala.util.Try$.apply(Try.scala:192)
	at org.apache.spark.streaming.scheduler.JobGenerator.generateJobs(JobGenerator.scala:247)
	at org.apache.spark.streaming.scheduler.JobGenerator.org$apache$spark$streaming$scheduler$JobGenerator$$processEvent(JobGenerator.scala:183)
	at org.apache.spark.streaming.scheduler.JobGenerator$$anon$1.onReceive(JobGenerator.scala:89)
	at org.apache.spark.streaming.scheduler.JobGenerator$$anon$1.onReceive(JobGenerator.scala:88)
	at org.apache.spark.util.EventLoop$$anon$1.run(EventLoop.scala:48)
```

* 解决思路：检查生产端和消费端参数
    - `session.timeout.ms`
    - `request.timeout.ms`

> 具体参数参考[官网文档](http://kafka.apache.org/documentation.html#consumerconfigs)