##########################################################################################
# ClassicDriver Docker image with Confluence installed
##########################################################################################
FROM classicdriver/java:1.8.60

MAINTAINER Team Nitrous <nitrous@classicdriver.com>

#############################################
# Confluence installation
#############################################
RUN apt-get update -qq \
    && apt-get clean autoclean \
    && apt-get autoremove --yes \
    && rm -rf /var/lib/{apt,dpkg,cache,log}/

ENV RUN_USER            confluence
ENV RUN_GROUP           confluence

ENV CONFLUENCE_HOME           /var/confluence_home
ENV CONFLUENCE_INSTALL_DIR    /opt/atlassian/confluence
ENV CONFLUENCE_VERSION        5.9.1

# Confluence is run with user `confluence`, uid = 1000
# If you bind mount a volume from the host or a data container,
# ensure you use the same uid
RUN useradd -c "confluence role account" -d "$CONFLUENCE_HOME" -u 1000 -m -s /bin/bash ${RUN_USER}

ENV DOWNLOAD_URL        https://www.atlassian.com/software/confluence/downloads/binary/atlassian-confluence-${CONFLUENCE_VERSION}.tar.gz

RUN set -x \
    && mkdir -p                           ${CONFLUENCE_INSTALL_DIR}                \
    && curl -L --silent                   ${DOWNLOAD_URL} | tar -xz --strip=1 -C "$CONFLUENCE_INSTALL_DIR" \
    && rm -rf                             atlassian-confluence-${CONFLUENCE_VERSION}.tar.gz  \
    && chmod -R 700                       ${CONFLUENCE_INSTALL_DIR}/conf           \
    && chmod -R 700                       ${CONFLUENCE_INSTALL_DIR}/logs           \
    && chmod -R 700                       ${CONFLUENCE_INSTALL_DIR}/temp           \
    && chmod -R 700                       ${CONFLUENCE_INSTALL_DIR}/work           \
    && chown -R ${RUN_USER}:${RUN_GROUP}  ${CONFLUENCE_INSTALL_DIR}/               \
    && chown -R ${RUN_USER}:${RUN_GROUP}  ${CONFLUENCE_HOME}/               \
    && echo                               "\nconfluence.home = $CONFLUENCE_HOME" >> "${CONFLUENCE_INSTALL_DIR}/confluence/WEB-INF/classes/confluence-init.properties"


USER ${RUN_USER}:${RUN_GROUP}

VOLUME ["${CONFLUENCE_HOME}"]

# HTTP Port
EXPOSE 8090

WORKDIR $CONFLUENCE_INSTALL_DIR

# Run in foreground
CMD ["/opt/atlassian/confluence/bin/start-confluence.sh", "-fg"]
