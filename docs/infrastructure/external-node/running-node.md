# Running Node

Before diving in, ensure that you have duly prepared a configuration file, as described in the [configuration](configuration.md) section.

### Hardware Specifications

Here's the recommended hardware setup, which is subject to further updates:

* **CPU**: 32-core
* **RAM**: 32GB
* **Storage (SSD)**:
  * **Testnet**: Approx. 800 GB (as of this writing). This is set to expand over time and necessitates routine monitoring.
  * **Mainnet**: Approx. 400 GB (as of this writing). Likewise, periodic monitoring is crucial.
  * **NVMe**: Strongly recommended for performance.
* **Network**: At least 100 Mbps connection.

#### Note on PostgreSQL Storage:

The most resource-intensive table in the database is the `call_traces` table. This table is pivotal for the `debug` namespace. If you aim to conserve storage and don't require the `debug` namespace, you can:

* Clear the table using the query: `DELETE FROM call_traces;`
* Disable the `debug` namespace through the `EN_API_NAMESPACES` environment variable, as illustrated in our configuration example.

### Infrastructure Setup

**1. PostgreSQL Server with SSD Storage**:

* **Testnet**: About 1TB (current estimate) – again, anticipate growth and monitor regularly.
* **Mainnet**: Roughly 2TB (current estimate) – monitor its growth.

While this guide doesn't delve deep into PostgreSQL setup, using Docker is a prevalent choice. There are ample guides available on this topic; here's [one](https://www.docker.com/blog/how-to-use-the-postgres-docker-official-image/) for your reference.

{% hint style="info" %}
**PostgreSQL as a standalone Docker**

If you choose to run PostgreSQL as a standalone Docker image (not via Docker-compose with a shared network between EN and PostgreSQL), the EN won't recognize PostgreSQL via localhost or 127.0.0.1 URLs. To resolve this, either:

* Operate it with a `--network host` (on Linux systems), or
* Substitute `localhost` with `host.docker.internal` in your EN configuration. More details are available in the \[official documentation]\[host\_docker\_internal].
{% endhint %}

**2. Database Dump**:

Secure a database dump matching your environment. This can be restored with the command:

`pg_restore -O -C <DUMP_PATH> --dbname=<DB_URL>`.

### Launching the Node

Given that you've secured the EN Docker image, an environment file featuring the prepared configuration, and have restored your database using the `pg_dump`, you're set to go.

**Example Command**:

```bash
docker run --env-file <path_to_env_file> --mount type=bind,source=<local_rocksdb_data_path>,target=<configured_rocksdb_data_path> <image>
```

Should you require Helm charts or other configuration options, they will be made accessible in due course.

TODO: add image

{% hint style="warning" %}
#### First-Time Startup

On the maiden launch, the state present in PostgreSQL will mirror the dump you employed, but RocksDB (primarily the Merkle tree) will be devoid of any state. The EN needs to reconstruct the state within RocksDB and affirm consistency. Depending on your hardware, anticipate a state rebuild duration on the mainnet **exceeding 20 hours**.
{% endhint %}

### Deploying EN with a Fresh PG Dump

If the EN has been operational for a while and you wish to redeploy using a fresh PG dump, follow these steps:

1. Cease EN operations.
2. Eliminate the State Keeper cache (as per `EN_STATE_CACHE_PATH`).
3. Delete your prevailing database.
4. Restore utilizing the new dump.
5. Reinitiate the EN.
