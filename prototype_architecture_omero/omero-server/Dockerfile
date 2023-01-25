# syntax=docker/dockerfile:1
FROM ubuntu:20.04
LABEL maintainer="ome-devel@lists.openmicroscopy.org.uk"


RUN mkdir /opt/setup
WORKDIR /opt/setup
ADD ./omero-server/playbook.yml /opt/setup/
ADD ./omero-server/requirements.yml /opt/setup/
ENV OMERODIR /opt/omero/server/OMERO.server
RUN apt update \
    && apt upgrade -y \
    #&& apt install -y epel-release \
    && apt install -y ansible curl sudo dumb-init \
    && ansible-galaxy install -p /opt/setup/roles -r requirements.yml \
    && apt -y autoclean \
    && apt -y autoremove \
    && rm -rf /var/lib/apt/lists/* /tmp/*

RUN ansible-playbook playbook.yml \
    && apt -y autoclean \
    && apt -y autoremove \
    && rm -rf /var/lib/apt/lists/* /tmp/*

#RUN curl -L -o /usr/local/bin/dumb-init \
    #https://github.com/Yelp/dumb-init/releases/download/v1.2.0/dumb-init_1.2.0_amd64 && \
    #chmod +x /usr/local/bin/dumb-init

#ADD ./omero-server/entrypoint.sh /usr/local/bin/
#RUN chmod ug+x /usr/local/bin/entrypoint.sh
#ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]

#ENTRYPOINT ["/bin/bash"]
#ENTRYPOINT ["/usr/bin/env python3"]

USER omero-server
EXPOSE 4063 4064
VOLUME ["/OMERO", "/opt/omero/server/OMERO.server/var"]
ENV ICE_HOME /opt/ice-3.6.5
ENV PATH "$ICE_HOME/bin:$PATH"
ENV LD_LIBRARY_PATH "$ICE_HOME/lib64:$ICE_HOME/lib:$LD_LIBRARY_PATH"
ENV SLICEPATH "$ICE_HOME/slice"
ENV OMERODIR /opt/omero/server/OMERO.server
WORKDIR /opt/omero/server/venv3/bin
CMD ["./omero", "admin", "diagnostics"]
