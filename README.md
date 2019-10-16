# chessmovedb

Pulls game from lichess.org, processes and perists them to disk. Lets the user
query a sequence of starting moves and suggests the next move to be played. This repository 
contains nothing; the application is made up of the components below.

## Components

* [chessmovedb-fetch](https://github.com/lsund/chessmovedb-fetch) for fetching gamedata from lichess
* [chessmovedb-store](https://github.com/lsund/chessmovedb-store) for persisting the games
* [chessmovedb-ui](https://github.com/lsund/chessmovedb-ui) the user interface (CLI)

The system uses Kafka for messaging. It produces and consumes from topics
`game`, `query` and `suggestion`.
