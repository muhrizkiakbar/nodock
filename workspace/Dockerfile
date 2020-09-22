FROM phusion/baseimage:0.9.19

RUN apt-get update && \
    apt-get install -y npm \
        mysql-client \
        sqlite3 \
        iputils-ping && \
    npm install -g n


##
## Timezone
##

ARG TZ=UTC
ENV TZ ${TZ}
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

##
## Node
##

ARG NODE_VERSION=latest

# Install the specified NODE_VERSION or grab latest
RUN n "$NODE_VERSION"

# Install YARN
RUN npm i -g yarn

##
## Cron
##

COPY ./crontab /var/spool/cron/crontabs

USER root

WORKDIR /opt/app

RUN chmod -R 777 /opt/app

# Add a non-root user to prevent files being created with root permissions on host machine.
ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

# always run apt update when start and after add new source list, then clean up at end.
RUN groupadd -g ${PGID} nodock && \
    useradd -u ${PUID} -g nodock -m nodock -G doker_env && \
    usermod -p "*" nodock

RUN chown -R nodock:nodock /opt/app

USER nodock

ENTRYPOINT sleep infinity
