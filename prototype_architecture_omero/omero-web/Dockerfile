# syntax=docker/dockerfile:1
FROM ubuntu:20.04
LABEL maintainer="ome-devel@lists.openmicroscopy.org.uk"


RUN mkdir /opt/setup
WORKDIR /opt/setup
ADD ./omero-web/playbook.yml /opt/setup/
ADD ./omero-web/requirements.yml /opt/setup/
ENV OMERODIR /opt/omero/web/OMERO.web/
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

#ADD ./omero-web/entrypoint.sh /usr/local/bin/
#RUN chmod ug+x /usr/local/bin/entrypoint.sh
#ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]

#ENTRYPOINT ["/bin/bash"]
#ENTRYPOINT ["/usr/bin/env python3"]

RUN chmod -R 777 /opt/omero/web

USER omero-web
EXPOSE 4080
VOLUME ["/opt/omero/web/OMERO.web/var"]
ENV OMERODIR /opt/omero/web/OMERO.web/
WORKDIR /opt/omero/web/venv3/bin
CMD ["./omero", "web", "diagnostics"]
