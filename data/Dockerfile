##########################################################################################
# ClassicDriver Confluence Data Volume Docker Image
##########################################################################################
FROM classicdriver/base

MAINTAINER Team Nitrous <nitrous@classicdriver.com>

# Confluence User
ENV RUN_USER            confluence

# Confluence Home
ENV CONFLUENCE_HOME     /var/confluence_home

RUN useradd -c "confluence role account" -d "$CONFLUENCE_HOME" -u 1000 -m -s /bin/bash ${RUN_USER}

VOLUME ["${CONFLUENCE_HOME}"]

# Switching user to 'confluence' (since this is a data valume and will be used by the confluence application container user)
USER ${RUN_USER}

# Command runned once at startup
CMD ["echo", "Data Volume Container for CONFLUENCE"]
