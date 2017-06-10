FROM debian:8

# set local vars
ENV container docker
ENV DEBIAN_FRONTEND noninteractive

# install dependencies
RUN sed -i 's/main/main contrib non-free/g' /etc/apt/sources.list
RUN echo "deb http://deb.debian.org/debian jessie-backports main contrib non-free" >> /etc/apt/sources.list
RUN apt-get update \
    && apt-get install -y -q systemd \
        curl \
        git \
        vim \
        sudo \
        locales \
        locales-all \
        cron \
        dbus \
        && apt-get clean \
        && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# add SaltStack repo
ADD conf/saltstack.list /etc/apt/sources.list.d/saltstack.list

# install SaltStack
RUN curl https://repo.saltstack.com/apt/debian/8/amd64/latest/SALTSTACK-GPG-KEY.pub | apt-key add - \
    && apt-get update \
    && apt-get install -y -q \
        salt-minion

# add minion's configuration modules
ADD conf/minion.d/* /etc/salt/minion.d/

RUN cd /lib/systemd/system/sysinit.target.wants/; ls | grep -v systemd-tmpfiles-setup | xargs rm -f $1 \
      rm -f /lib/systemd/system/multi-user.target.wants/*;\
      rm -f /etc/systemd/system/*.wants/*;\
      rm -f /lib/systemd/system/local-fs.target.wants/*; \
      rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
      rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
      rm -f /lib/systemd/system/basic.target.wants/*;\
      rm -f /lib/systemd/system/anaconda.target.wants/*; \
      rm -f /lib/systemd/system/plymouth*; \
      rm -f /lib/systemd/system/systemd-update-utmp*;

#boot non-GUI
RUN systemctl set-default multi-user.target

# set local vars
ENV init /lib/systemd/systemd
ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8

VOLUME [ "/sys/fs/cgroup" ]

ENTRYPOINT ["/lib/systemd/systemd"]
