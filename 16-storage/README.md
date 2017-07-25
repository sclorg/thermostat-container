Thermostat Storage Docker image
=============================

This repository contains Dockerfiles for a Thermostat Storage running in privileged mode
that can be accessed by Clients and Agents through HTTP.

Environment variables
---------------------------------

The image recognizes the following environment variables that you can set during
initialization by passing `-e VAR=VALUE` to the Docker run command.

|    Variable name               |    Description                              |
| :----------------------------- | -----------------------------------------   |
|  `THERMOSTAT_AGENT_USERNAMES`  | Space separated list of user names for Thermostat agents to connect to storage |
|  `THERMOSTAT_AGENT_PASSWORDS`  | Space separated list of passwords for agents connecting to storage   |
|  `THERMOSTAT_CLIENT_USERNAMES` | Space separated list of user names for Thermostat clients to connect to storage |
|  `THERMOSTAT_CLIENT_PASSWORDS` | Space separated list of passwords for clients connecting to storage  |
|  `MONGO_USERNAME`              | User name for the mongodb backing storage   |
|  `MONGO_PASSWORD`              | Password for the mongodb backing storage    |
|  `MONGO_URL`                   | Mongodb URL to connect to                   |

Usage
---------------------------------

For this, we will assume that you are using the `rhscl/thermostat-16-storage-rhel7` image.
To run Thermostat Storage connected to some other mongodb backend (e.g. provided by
another container) supply the mongodb url, mongo username/passwords and the agent/client
usernames/passwords by executing the following command:

```
$ docker run -d \
             -e MONGO_URL=mongodb://172.17.0.1:27017 \
             -e MONGO_USERNAME=mongouser \
             -e MONGO_PASSWORD=mongopass \
             -e THERMOSTAT_AGENT_USERNAMES="agentuser1 agentuser2" \
             -e THERMOSTAT_AGENT_PASSWORDS="agentpass1 agentpass2" \
             -e THERMOSTAT_CLIENT_USERNAMES="clientuser1 clientuser2" \
             -e THERMOSTAT_CLIENT_PASSWORDS="clientpass1 clientpass2" \
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

In other words, the above example creates the following agent users:

- `agentuser1` with password `agentpass1`
- `agentuser2` with password `agentpass2`

Either one of them, or both, can be used for Thermostat agent connections.

Moreover, the following client users will be usable:

- `clientuser1` with password `clientpass1`
- `clientuser2` with password `clientpass2`

Either one of them, or both, can be used for Thermostat client connections.
