# chessmovedb

This project is experimental, probably useless.

## What is this?

A distributed system which pulls chess game data from lichess.org, processes and
builds a database of moves. Lets the user query a sequence of starting moves and
suggests the next move to be played based on the data in the database. To see
the actual code, check out the components below.

## Why?

I wanted to get my hands dirty with scala and Kafka. I did not have much prior
knowledege with any of those technologies.

## Components

* [chessmovedb-fetch](https://github.com/lsund/chessmovedb-fetch) for fetching gamedata from lichess
* [chessmovedb-store](https://github.com/lsund/chessmovedb-store) for persisting
  the games to disk
* [chessmovedb-ui](https://github.com/lsund/chessmovedb-ui) the user interface (CLI)

The system uses Kafka for messaging. It produces and consumes from topics
`game`, `query` and `suggestion`.

## Usage

The system needs a kafka instance running on `localhost:9092` and zookeeper
running on `localhost:2181`

Make sure you have [Kafka](https://kafka.apache.org/) installed.

```shell
zkServer.sh start
/usr/bin/kafka-server-start.sh -daemon /etc/kafka/server.properties
# Create the topic
/usr/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic game
/usr/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic query
/usr/bin/kafka-topics.sh --create --zookeeper localhost:2181
--replication-factor 1 --partitions 1 --topic suggestion
```

#### Shell 1

```shell
git clone https://github.com/lsund/chessmovedb-fetch
sbt run
```

#### Shell 2

```shell
git clone https://github.com/lsund/chessmovedb-store
sbt run
```
#### Shell 3

```shell
git clone https://github.com/lsund/chessmovedb-fetch
sbt clean assembly
java -jar target/scala-2.12/chessmovedb-ui-assembly-1.0.0.jar --moves 'e4 d6 d4'
```
