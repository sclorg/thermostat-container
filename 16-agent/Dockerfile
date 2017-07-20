FROM centos:centos7
# Thermostat Agent Image.
#
# Volumes:
#  * /opt/rh/rh-thermostat16/root/usr/share/thermostat
# Environment:
#  * $THERMOSTAT_CMDC_PORT      - The command channel listen port.
#  * $THERMOSTAT_CMDC_ADDR      - The command channel listen address.
#  * $THERMOSTAT_AGENT_USERNAME - User name for the thermostat agent to use
#                                 for connecting to storage.
#  * $THERMOSTAT_AGENT_PASSWORD - Password for the thermostat agent to use
#                                 for connecting to storage.
#  * $THERMOSTAT_DB_URL         - The storage URL to connect to.

ENV THERMOSTAT_VERSION=1.6 \
    HOME=/root

ENV SUMMARY="A monitoring and serviceability tool for OpenJDK."	\
    DESCRIPTION="The powerful, free and open source instrumentation tool for the Hotspot JVM."

LABEL summary="$SUMMARY" \
      description="$DESCRIPTION" \
      io.k8s.description="$SUMMARY" \
      io.k8s.display-name="Thermostat Agent 1.6" \
      com.redhat.component="rh-thermostat16-agent-docker" \
      name="centos/thermostat-16-agent-centos7" \
      version="1.6" \
      release="1"

ENV THERMOSTAT_CMDC_ADDR 127.0.0.1
ENV THERMOSTAT_CMDC_PORT 12000
ENV THERMOSTAT_DB_URL http://127.0.0.1:8999/thermostat/storage
ENV THERMOSTAT_HOME /opt/rh/rh-thermostat16/root/usr/share/thermostat
ENV THERMOSTAT_JARS /opt/rh/rh-thermostat16/root/usr/share/java/thermostat
ENV USER_THERMOSTAT_HOME /root/.thermostat-1.6

EXPOSE $THERMOSTAT_CMDC_PORT

RUN yum install -y yum-utils centos-release-scl-rh && \
    yum-config-manager --enable centos-sclo-rh-testing && \
    INSTALL_PKGS="rh-thermostat16" && \
    yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum erase -y java-1.8.0-openjdk-headless && \
    yum clean all

COPY thermostat-user-home-config ${USER_THERMOSTAT_HOME}
COPY root /

# Get prefix path and path to scripts rather than hard-code them in scripts
ENV CONTAINER_SCRIPTS_PATH=/usr/share/container-scripts/thermostat \
    ENABLED_COLLECTIONS=rh-thermostat16

# When bash is started non-interactively, to run a shell script, for example it
# looks for this variable and source the content of this file. This will enable
# the SCL for all scripts without need to do 'scl enable'.
ENV BASH_ENV=${CONTAINER_SCRIPTS_PATH}/scl_enable \
    ENV=${CONTAINER_SCRIPTS_PATH}/scl_enable \
    PROMPT_COMMAND=". ${CONTAINER_SCRIPTS_PATH}/scl_enable"

# For the agent vm-profiler to work thermostat jars need to be available to other containers
# using the volumes from this container.
VOLUME ${THERMOSTAT_JARS}
VOLUME ${THERMOSTAT_HOME}

WORKDIR ${THERMOSTAT_HOME}

ENTRYPOINT ["container-entrypoint"]
CMD [ "run-thermostat-agent" ]
