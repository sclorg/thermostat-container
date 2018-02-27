Thermostat Agent and Storage Docker images
==========================================

This repository contains Dockerfiles for Thermostat Agent and Storage Docker images.
Users can choose between RHEL and CentOS based images.


Versions
---------------
**All of the Thermostat images are deprecated**

Installation
---------------
To build a Thermostat image, choose either the CentOS or RHEL based image:
*  **RHEL based image**

    This image is available in Red Hat Container Registry. To download it run:

    ```
    $ docker pull registry.access.redhat.com/rhscl/thermostat-16-storage-rhel7
    ```

    To build a RHEL based Thermostat image, you need to run the build on a properly
    subscribed RHEL machine.

    ```
    $ git clone --recursive https://github.com/sclorg/thermostat-container.git
    $ cd thermostat-container
    $ git submodule update --init
    $ make build TARGET=rhel7 VERSIONS=1.6-agent
    ```

*  **CentOS based image**

    This image is available on DockerHub. To download it run:

    ```
    $ docker pull centos/thermostat-16-storage-centos7
    ```

    To build a Thermostat image from scratch run:

    ```
    $ git clone --recursive https://github.com/sclorg/thermostat-container.git
    $ cd thermostat-container
    $ git submodule update --init
    $ make build TARGET=centos7 VERSIONS=1.6-agent
    ```

**Notice: By omitting the `VERSIONS` parameter, the build/test action will be performed
on all provided versions of Thermostat.**


Usage
---------------------------------

For information about usage of Dockerfile for Thermostat 1.6 Agent,
see [usage documentation](1.6-agent/README.md).

Test
---------------------
Users can choose between testing a Thermostat test application based on a RHEL or CentOS image.

*  **RHEL based image**

    To test a RHEL7 based Thermostat image, you need to run the test on a properly
    subscribed RHEL machine.

    ```
    $ cd thermostat-container
    $ git submodule update --init
    $ make test TARGET=rhel7 VERSIONS=1.6-agent
    ```

*  **CentOS based image**

    ```
    $ cd thermostat-container
    $ git submodule update --init
    $ make test TARGET=centos7 VERSIONS=1.6-agent
    ```

**Notice: By omitting the `VERSIONS` parameter, the build/test action will be performed
on all provided versions of Thermostat.**
