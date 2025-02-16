---
title: Upgrading to InfluxDB 1.7.x

menu:
    influxdb_1_7:
        name: Upgrading
        weight: 25
        parent: Administration
---


We recommend enabling Time Series Index (TSI) (step 3 of Upgrade to InfluxDB 1.7.x). [Switch between TSM and TSI](#switch-between-tsm-and-tsi-indexes) as needed. To learn more about TSI, see:

- [Time Series Index (TSI) overview](/influxdb/v1.7/concepts/time-series-index/)
- [Time Series Index (TSI) details](/influxdb/v1.7/concepts/tsi-details/)

> **_Note:_** The default configuration continues to use TSM-based shards with in-memory indexes (as in earlier versions).

## Upgrade to InfluxDB 1.7.x

1. [Download](https://portal.influxdata.com/downloads) InfluxDB version 1.7.x and [install the upgrade](/influxdb/v1.7/introduction/installation).

2. Migrate configuration file customizations from your existing configuration file to the InfluxDB 1.7.x [configuration file](/influxdb/v1.7/administration/config/). Add or modify your environment variables as needed.

3. To enable TSI in InfluxDB 1.7.x, complete the following steps:

    a. If using the InfluxDB configuration file, find the `[data]` section, uncomment `index-version = "inmem"` and change the value to `tsi1`.

    b. If using environment variables, set `INFLUXDB_DATA_INDEX_VERSION` to `tsi1`.

    c. Delete shard `index` directories (by default, located at `/<shard_ID>/index`).

    d. Convert TSM-based shards to TSI-based shards by running the [influx_inspect buildtsi](/influxdb/v1.7/tools/influx_inspect/#buildtsi) command.

    > **Note** Run the buildtsi command using the user account that you are going to run the database as, or ensure that the permissions match afterward.

4. Restart the `influxdb` service.

## Switch between TSM and TSI indexes

After upgrading to InfluxDB 1.7.x, switch between using the TSM index and the TSI index as needed.

### To switch from TSM to TSI

Complete step 3 and 4 in [Upgrade to InfluxDB 1.7.x](#upgrade-to-influxdb-1-7-x).

### To switch from TSI to TSM

1. In the InfluxDB configuration file, set `index-version = "inmem"`. 
2. Delete all shard `index` directories (by default, located at `/<shard_ID>/index`).
3. Restart the `influxdb` service.

## Upgrade InfluxDB Enterprise clusters

See [Upgrading InfluxDB Enterprise clusters](/enterprise_influxdb/v1.7/administration/upgrading/).
