FROM centos/s2i-base-centos7
# Thermostat Agent Image.
#
# Environment:
#  * $THERMOSTAT_AGENT_USERNAME - User name for the thermostat agent to use
#                                 for connecting to storage.
#  * $THERMOSTAT_AGENT_PASSWORD - Password for the thermostat agent to use
#                                 for connecting to storage.
#  * $THERMOSTAT_DB_URL         - The storage URL to connect to.
#  * $APP_USER                  - The application user the Java app Thermostat
#                                 shall monitor runs as.

ENV THERMOSTAT_VERSION=1.6 \
    APP_USER="default"

ENV SUMMARY="A monitoring and serviceability tool for OpenJDK."	\
    DESCRIPTION="The powerful, free and open source instrumentation tool for the Hotspot JVM."

LABEL summary="$SUMMARY" \
      description="$DESCRIPTION" \
      io.k8s.description="$DESCRIPTION" \
      io.k8s.display-name="Thermostat Agent 1.6" \
      com.redhat.component="rh-thermostat16-agent-docker" \
      name="centos/thermostat-16-agent-centos7" \
      version="1.6" \
      release="1"

ENV THERMOSTAT_CMDC_PORT 12000
ENV THERMOSTAT_HOME /opt/rh/rh-thermostat16/root/usr/share/thermostat
ENV THERMOSTAT_JARS /opt/rh/rh-thermostat16/root/usr/share/java/thermostat
ENV USER_THERMOSTAT_HOME /opt/app-root/.thermostat-1.6

EXPOSE ${THERMOSTAT_CMDC_PORT}

# Create USER_THERMOSTAT_HOME parent folder, ensure any UID can write to it.
RUN chown -R default:0 /opt/app-root && \
    chmod g+x /home && chmod g+wx /opt/app-root

RUN yum install -y yum-utils centos-release-scl-rh && \
    yum-config-manager --enable centos-sclo-rh-testing && \
    INSTALL_PKGS="rh-thermostat16 nss_wrapper" && \
    yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all

COPY thermostat-user-home-config ${USER_THERMOSTAT_HOME}
COPY root /

# Ensure any UID can read/write to files in /opt/app-root
RUN chown -R default:0 /opt/app-root && \
    find /opt/app-root -type d -exec chmod g+rwx '{}' \; && \
    find /opt/app-root -type f -exec chmod g+rw '{}' \;

WORKDIR ${HOME}

USER 1001

# Get prefix path and path to scripts rather than hard-code them in scripts
ENV CONTAINER_SCRIPTS_PATH=/usr/share/container-scripts/thermostat \
    ENABLED_COLLECTIONS=rh-thermostat16

# When bash is started non-interactively, to run a shell script, for example it
# looks for this variable and source the content of this file. This will enable
# the SCL for all scripts without need to do 'scl enable'.
ENV BASH_ENV=${CONTAINER_SCRIPTS_PATH}/scl_enable \
    ENV=${CONTAINER_SCRIPTS_PATH}/scl_enable \
    PROMPT_COMMAND=". ${CONTAINER_SCRIPTS_PATH}/scl_enable"

ENTRYPOINT ["container-entrypoint"]
CMD [ "run-thermostat-agent" ]
