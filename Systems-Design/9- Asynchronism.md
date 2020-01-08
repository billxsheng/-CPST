# Asynchronism

![](https://camo.githubusercontent.com/c01ec137453216bbc188e3a8f16da39ec9131234/687474703a2f2f692e696d6775722e636f6d2f353447597353782e706e67)

- Async workflows help reduce request times by performing expensive operations that would otherwise be performed in-line
- They can also perform time-consuming work ahead of time, such as periodic aggregation of data

## Message Queues 
- Receive, hold, and deliver messages
- If an operation is too slow to perform inline, you can use a message queue by:
    - App publishes a job to the queue and notifies the user of job status
    - Worker/agent picks up the job from the queue, processes it, and notifies when the job is complete
- The user is not blocked and the job is processed in the background
- Redis is useful as a simple message broker but messages can be lost
- RabbitMQ is popular but requires you to adapt to AMQP protocol and manage nodes

### Queuing 
- A pool of consumers may read from a server and each record goes to ONE of them
- Queuing allows you to divide up the processing of data over multiple consumer instances (scalable)
- Are not multi-subscriber
### Pub/Sub
- Any message published to a topic is immediately received by all of the subscribers to the topic
- The record is broadcast to all consumers
- Allows you to broadcast data to multiple processes, but no way of scaling process since every message goes to every subscriber

### Kafka
- distributed streaming platform
- Popular because it includes the broker itself (brokers receive messages from producers and stores them on disk keyed by an offset)
- Messaging: Each consumer is assigned to one topic partition so that data is in order and processing occurs in-parallel 
- Storage: Data is written to disk and replicated, Kafka disk structures also scale well 
- Streaming: Producer and Consumer APIs allow for simple processing
    - Streams API can be used for complex transformations (like Spark)
- By combining storage and subscriptions, streaming apps can treat both past and future data the same way
    - Historical data can be processed and can continue once it reaches the end
    - Includes batch processing 
- Ability to store data makes it possible to use Kafka for critical data where delivery must be guaranteed or for integration with systems that load data periodically 
    - Can combine real-time events with subscriptions

## Task Queues
- Receives tasks, runs them, delivers results
- Support scheduling and can used to run intensive jobs in the background

## Back Pressure
- If queues grow, the size can be larger than memory
- results in cache misses, disk reads, and slow performance
- back pressure can limit queue size
    - maintains high throughput rate
    - Good response times
- When a queue fills up, clients get a server busy or 503 status code to try later

## Disadvantages
- Use cases such as inexpensive calculations and realtime workflows might be better suited for sync operations as queues can add delays and complexity 