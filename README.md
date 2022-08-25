## Postgres + Debezium & Timescale

This is intended as a lightweight Docker image using Postgres 14, Debezium pre-requisites and TimescaleDB.

This also includes PostGIS support.

By default, it will run the Timescaledb tuner as part of the initialisation process.

Taken from this source Docker container:

* https://github.com/debezium/container-images/tree/main/postgres/14

The TimescaleDB extension is installed by following the guide from here:

* https://docs.timescale.com/install/latest/self-hosted/installation-debian/#set-up-the-timescaledb-extension


## Debezium info

The [Postgres](https://www.postgresql.org) relational database management system has a feature called [logical decoding](https://www.postgresql.org/docs/14/static/logicaldecoding-explanation.html) that allows clients to extract all persistent changes to a database's tables into a coherent, easy to understand format which can be interpreted without detailed knowledge of the database's internal state. An output plugin transform the data from the write-ahead log's internal representation into the format the consumer of a replication slot desires.

This image is based upon [`postgres:14`](https://hub.docker.com/_/postgres/) and adds two logical decoding plug-ins:

* [postgres-decoderbufs](https://github.com/debezium/), based on Protocol Buffers and maintained by the Debezium community
* [wal2json](https://github.com/eulerto/wal2json), based on JSON and maintained by the wal2json community

Both are supported by the [Debezium PostgreSQL Connector](http://debezium.io/docs/connectors/postgresql/) to capture changes committed to the database and record the data change events in Kafka topics.
In addition, Debezium supports the `pgoutput` plug-in, which is available by default on Postgres 10 and later.
The image also includes [PostGIS](http://www.postgis.net) spatial database extender used to provide geospatial queries, so that changes to geometric data can also be captured by Debezium.
