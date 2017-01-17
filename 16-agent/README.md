Thermostat Agent Docker image
=============================

This repository contains Dockerfiles for a Thermostat Agent base image which can be used
for Java application images or builder images to be based on it. By basing your image on the
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

This image is intended to be used as a base image for builder/runtime images in your
Dockerfile via:

    FROM rhscl/thermostat-16-agent-rhel7

For example, a complete drop-in replacement Dockerfile for the `openshift/wildfly-101-centos7`
image which will have the Thermostat agent pre-installed has been illustrated here:

https://github.com/jerboaa/thermostat-agent-container-ex

Once the `rhscl/thermostat-16-agent-rhel7` image has been introduced into the image
hierarchy the Thermostat agent can be started by setting the three required Thermostat
environment variables. Let's consider image `rhscl/thermostat-test` which uses
`rhscl/thermostat-16-agent-rhel7` as its base image and runs Java app `foo` on deployment,
then a Thermostat agent can be started together with `foo` by setting the following
environment variables:

    THERMOSTAT_AGENT_USERNAME
    THERMOSTAT_AGENT_PASSWORD
    THERMOSTAT_DB_URL

Image `rhscl/thermostat-16-storage-rhel7` can be used to set up a storage endpoint for the
agent to connect to.
