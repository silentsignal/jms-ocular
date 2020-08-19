Ocular JMS Queries
==================

This is a collection of [CPGQL](https://docs.shiftleft.io/ocular/quickstart) queries for [Ocular](https://ocular.shiftleft.io/) to help the identification potentially dangerous uses of JMS implementations.

Introductory blog post: https://blog.silentsignal.eu/2020/08/17/unexpected-deserialization-pt-1-jms/

Call chains to potential sinks were queried using the following query (aimed at `ObjectInputStream`):

```
val sinks = cpg.method.fullName("java.io.ObjectInputStream.readObject.*")
val sources=cpg.typ.fullName("javax.jms.Message").derivedType.method
sinks.calledBy(sources).newCallChain.l
```

Contributions are welcome!

IBM MQ
------

Analyzed version: 9.2.0.0

```
cpg.typ.fullName("javax.jms.Message").derivedType.method.or(_.name("getBody"), _.name("getObject"),_.name("getObjectInternal")).l
```

HornetMQ
--------

Analyzed version: 2.4.7

```
cpg.typ.fullName("javax.jms.Message").derivedType.method.or(_.name("getBody"), _.name("getBodyInternal")).l
```

RabbitMQ
--------

Analyzed version: 1.4.7

```
cpg.typ.fullName("javax.jms.Message").derivedType.method.or(_.name("fromMessage"), _.name("convertJmsMessage")).l
```
