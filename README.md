# amqp
A draft version for AMQP wrapper

# Maven dependency
```
<dependency>
    <groupId>net.kut3.mq</groupId>
    <artifactId>amqp</artifactId>
    <version>0.1.5</version>
</dependency>
```

#  Configuration
## Default connection factory
```
[amq]
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
exchanges = queue01,queue02

[queue01-queue]
durable = false
bind-exchange = exchangeName
bind-key = routingKey
```
