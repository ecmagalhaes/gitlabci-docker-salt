FROM debian:8

# install dependencies
RUN apt-get update \
    && apt-get install -y -q \
        curl \
        git \
        vim \
        sudo \
        locales \
        locales-all \
        cron

# add SaltStack repo
ADD conf/saltstack.list /etc/apt/sources.list.d/saltstack.list

# install SaltStack
RUN curl https://repo.saltstack.com/apt/debian/8/amd64/latest/SALTSTACK-GPG-KEY.pub | apt-key add - \
    && apt-get update \
    && apt-get install -y -q \
        salt-minion

# set local vars
ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8

# Set the timezone.
ENV TZ Europe/Berlin
RUN echo $TZ > /etc/timezone && \
    rm /etc/localtime && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata

# add minion's configuration modules
ADD conf/minion.d/* /etc/salt/minion.d/
