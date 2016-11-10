Thermostat Agent Docker image
=============================

This repository contains Dockerfiles for a Thermostat Agent base image which can be used
for Java application images or builder images to be based on. By basing your image on the
`rhscl/thermostat-16-agent-rhel7` image, a Thermostat agent can get enabled on demand in
order to monitor your Java app.

Environment variables
---------------------------------

The image recognizes the following environment variables that you can set during
initialization by passing `-e VAR=VALUE` to the Docker run command.

|    Variable name              |    Description                              |
| :---------------------------- | -----------------------------------------   |
|  `THERMOSTAT_AGENT_USERNAME`  | User name for the Thermostat agent to use connecting to storage |
|  `THERMOSTAT_AGENT_PASSWORD`  | Password for connecting to storage          |
|  `THERMOSTAT_DB_URL`          | The URL for Thermostat storage              |
|  `APP_USER`                   | The application user the Java app Thermostat shall monitor runs as |


Usage
---------------------------------

TODO (write a tutorial what is needed for Dockerfiles to consume the image)
