# amqp
A draft version for AMQP wrapper.

# Maven dependency
```
<dependency>
    <groupId>net.kut3.mq</groupId>
    <artifactId>amqp</artifactId>
    <version>0.1.7</version>
</dependency>
```
# Basic usecases
## Initialize connection factory
#### With implicit configuration section name
```
ConnectionFactory connectionFactory = new ConnectionFactory();
```
#### With explicit configuration section name
```
ConnectionFactory connectionFactory = new ConnectionFactory(configSectionName);
```
## Produce (send) a message 
Before sending a message we must:
- Declare a queue.
- Declare an exchange.
   - Default exchange ("") doesn't need to be declared.
   - Sending mode is declared as exchange type. Supporting sending modes:
      - direct: send one message to one receiver
      - fanout: send one message to many receivers
- Bind the declared queue to the declared exchange by a routing key.
   - By default, all new queues will be bound to default exchange with the routing key value is the same as the queue name.

This library supports these steps through configuration (See the Configuration section for details)
#### Initialize a producer
```
Producer producer = connectionFactory.newProducer();
```
#### Send a direct messsage
```
producer.routingKey(queueName)
        .produce(new Message("Direct message"));
```
#### Send a fanout messsage
```
producer.exchange(exchangeName)
        .produce(new Message("Fanout message"));
```
#### Close ununsed producer
A producer owns one connection so we must close it after use to release the connection by calling the `close()` method. Keep in mind, in most cases, you only need one producer for one receiver (reusability) and let the runtime environment closes it implicit on shutdown step by calling the `closeOnExit()` method in producer initialization step.
## Consume (Receive) messages
#### Initialize a consumer (receiver)
```
Consumer consumer = connectionFactory.newConsumer()
                                     .closeOnExit();
```
#### Consume messsages
```
consumer.queue(queueName)
        .consume(m -> System.out.println(m));
```
#  Configuration
## Connection factory
#### Implicit configuration section
```
[amq]
uri = amqp://user:password@host:port/virtualhost
```
#### Explicit configuraction section
```
[custom-name]
uri = amqp://user:password@host:port/virtualhost
```
## Automatic exchange(s) declaration
#### Default exchange declaration (type=direct, durable=true)
```
[amq]
uri = amqp://user:password@host:port/virtualhost
exchanges = exchange01,exchange02
```
#### Explicit exchange declaration
```
[amq]
uri = amqp://user:password@host:port/virtualhost
exchanges = exchange01,exchange02

[exchange01-exchange]
type = fanout
durable = false
```
## Automatic queue(s) declaration
#### Default queue declaration (durable=true, binding-exchange="", routingKey=queueName)
```
[amq]
uri = amqp://user:password@host:port/virtualhost
queues = queue01,queue02
```
#### Explicit queue declaration
```
[amq]
uri = amqp://user:password@host:port/virtualhost
queues = queue01,queue02

[queue01-queue]
durable = false
binding-exchange = exchangeName
routing-key = routingKey
```
