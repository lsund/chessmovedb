# chessmovedb

This project is experimental, probably useless.

## What is this?

A distributed system which pulls chess game data from lichess.org, processes and
builds a database of moves. Lets the user query a sequence of starting moves and
suggests the next move to be played based on the data in the database. To see
the actual code, check out the components below.

## Why?

I wanted to get my hands dirty with scala and Kafka. I did not have much prior
knowledge with any of those technologies.

## Components

* [chessmovedb-fetch](https://github.com/lsund/chessmovedb-fetch) for fetching game data from lichess
* [chessmovedb-store](https://github.com/lsund/chessmovedb-store) for persisting
  the games to disk
* [chessmovedb-ui](https://github.com/lsund/chessmovedb-ui) the user interface (CLI)

The system uses Kafka for messaging. It produces and consumes from topics
`game`, `query` and `suggestion`.

## Test Usage

* Make sure you have [Kafka](https://kafka.apache.org/) installed. The kafka
  instance should be running on `localhost:9092` and the zookeeper instance
  on `localhost:2181`
* Create kafka topics: `game`, `query` and `suggestion`
* Start zookeeper and the kafka server
* Create an empty postgres database `chessgame`

Now, start the game fetcher:

```shell
git clone https://github.com/lsund/chessmovedb-fetch
cd chessmovedb-fetch
sbt run
```

`chessmovedb-fetch` now repeatedly asks for the best game on `lichess.org/tv`
(games played in real time) waits until the games are finished and then
downloads them. One game should be downloaded every 2-10 minutes, so you don't
have to be afraid that this program floods your hard-disk. Now start the game storer:

```shell
git clone https://github.com/lsund/chessmovedb-store
cd chessmovedb-store
sbt run
```

`chessmovedb-store` processes the downloaded games, and stores them in
postgres. Now build the cli interface:

```shell
git clone https://github.com/lsund/chessmovedb-ui
cd chessmovedb-ui
sbt clean assembly
```

The ui component is a command line tool, we can test it by invoking the jar
directly. The following command would ask the system for the next move given
that e4, d6 and d4 have been played.

```shell
java -jar target/scala-2.12/chessmovedb-ui-assembly-1.0.0.jar --moves 'e4 d6 d4'
```
