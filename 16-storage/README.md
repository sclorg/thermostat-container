Thermostat Storage Docker image
=============================

This repository contains Dockerfiles for a Thermostat Storage running in privileged mode
that can be accessed by Clients and Agents through HTTP.

Environment variables
---------------------------------

The image recognizes the following environment variables that you can set during
initialization by passing `-e VAR=VALUE` to the Docker run command.

|    Variable name              |    Description                              |
| :---------------------------- | -----------------------------------------   |
|  `THERMOSTAT_AGENT_USERNAME`  | User name for Thermostat agents to connect to storage |
|  `THERMOSTAT_AGENT_PASSWORD`  | Password for agents connecting to storage   |
|  `THERMOSTAT_CLIENT_USERNAME` | User name for Thermostat clients to connect to storage |
|  `THERMOSTAT_CLIENT_PASSWORD` | Password for clients connecting to storage  |
|  `MONGO_USERNAME`             | User name for the mongodb backing storage   |
|  `MONGO_PASSWORD`             | Password for the mongodb backing storage    |
|  `MONGO_URL`                  | Mongodb URL to connect to                   |

Usage
---------------------------------

For this, we will assume that you are using the `rhscl/thermostat-16-storage-rhel7` image.
To run Thermostat Storage connected to some other mongodb backend (e.g. provided by
another container) supply the mongodb url, mongo username/passwords and the agent/client
username/passwords by executing the following command:

```
$ docker run -d \
             -e MONGO_URL=mongodb://172.17.0.1:27017 \
             -e MONGO_USERNAME=mongouser \
             -e MONGO_PASSWORD=mongopass \
             -e THERMOSTAT_AGENT_USERNAME=agentuser \
             -e THERMOSTAT_AGENT_PASSWORD=agentpass \
             -e THERMOSTAT_CLIENT_USERNAME=clientuser \
             -e THERMOSTAT_CLIENT_PASSWORD=clientpass \
             --name thermostat16-storage \
             rhscl/thermostat-16-storage-rhel7
```

This will run a container with the http layer connected to the mongodb url using
the supplied mongo username and password. This can be accessed via:
http://ip-address:8080/thermostat/storage with the appropriate client or
agent credentials that you specified with the environment variables. To find the
ip address you can run:

```
$ docker inspect --format '{{ .NetworkSettings.IPAddress }}' thermostat16-storage
```
