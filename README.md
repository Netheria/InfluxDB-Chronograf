# InfluxDB-Chronograf
Stack template for storage and monitoring to use with JMeter

* [InfluxDB](https://github.com/influxdata/influxdb) - time series database storage
* [Chronograf](https://github.com/influxdata/chronograf) - allows you to quickly see the data that you have stored in InfluxDB and allows you to rapidly build dashboards with real-time visualizations of your data.

## Ports

The services in the app run on the following ports:

| Host Port | Service |
| - | - |
| 8086 | InfluxDB internal |
| 8087 | InfluxDB external |
| 8888 | Chronograf |

## How to start?

1. Install [Docker](https://docs.docker.com/engine/install/) and [docker compose](https://docs.docker.com/compose/install/)
> (Mac, Win, Linux) Docker Desktop: If you have Desktop installed then you already have the Compose plugin installed.
2. Clone this repo
3. Docker Compose:
   - For Docker Compose V1 run the following command:
```
docker-compose up -d
```
   - For Docker Compose V2 (if you enabled it in the Docker setting) you can run command in the following form:
```
docker compose up -d
```
4. Find InfluxDB container ip address for internal connection with Chronograf:
```
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' influxdb
```
5. Enter Chronograf UI here: http://localhost:8888/
6. InfluxDB connection:
   - Use InfluxDB's container ip address found at step **4** at **Connection URL** field
   > If not, you will receive **unable to connect to influxdb influx 1 error contacting source** error
   - Fill in Username and Password: admin\admin
   - Delete Telegraf Database Name field 
   > if you won't use Telegraf ofc
7. In JMeter add Backend Listener:
   - Backend Listener Implementation: InfluxdbBackendListenerClient
   - influxdbUrl: http://127.0.0.1:8087/write?db=db0
   > as mentioned before, it is yours external connection
   > Docker Compose file is configured to create **db0** database
8. Create dashboards, start tests, and enjoy the show!

## Environment variables

You are free to change InfluxDB's Username, Password, Database name in .env file.