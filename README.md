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

* Make sure you have [Kafka](https://kafka.apache.org/) installed.
* Create kafka topics: `game`, `query` and `suggestion`
* Start zookeeper and the kafka server
* Create a postgres database `chessgame`

Now, start the game fetcher

```shell
git clone https://github.com/lsund/chessmovedb-fetch
cd chessmovedb-fetch
sbt run
```

`chessmovedb-fetch` now repeatedly asks for the best game on `lichess.org/tv`,
waits until its done and then downloads it.

```shell
git clone https://github.com/lsund/chessmovedb-store
cd chessmovedb-fetch
sbt run
```

`chessmovedb-store` processses the downloaded games, and stores them in
postgres.

#### Shell 3

```shell
git clone https://github.com/lsund/chessmovedb-ui
cd chessmovedb-fetch
sbt clean assembly
```

The ui component has a command line interface, we can test it by invoking the
jar. This command would ask the system for the next move given that e4, d6 and
`d4` have been played.

```shell
java -jar target/scala-2.12/chessmovedb-ui-assembly-1.0.0.jar --moves 'e4 d6 d4'
```
