# GitLabCI-Docker-Salt

This repository contains a set of Docker images that install and configures
SaltStack in use with GitLab-CI. Available Dockers are based on different
distributions:

* CentOS 7 ([`Dockerfile`](./centos/7/Dockerfile))
* Debian 8 ([`Dockerfile`](./debian/8/Dockerfile))


## GitLab CI

This Docker images are used for continuous integration.

* [Getting started with GitLab CI](https://docs.gitlab.com/ce/ci/quick_start/README.html)
* [GitLab Runner ](https://docs.gitlab.com/runner/)

Configuration file:

* [`gitlab-ci.yml`](./.gitlab-ci.yml)

Copying the actual build to the Docker Image:

     sudo cp -R /builds/SaltStack/salt /srv/
     sudo cp -R /builds/SaltStack/pillar /srv/

Starting Salt:

    sudo salt-call --local state.apply --retcode-passthrough --force-color

Advanced configuration:

* [`config.toml`](./config.toml)


## Container Registry

GitLab Container Registry is a secure and private registry for Docker images which can be used by the GitLab CI.

* [GitLab Container Registry](https://docs.saltstack.com/en/latest/topics/tutorials/quickstart.html)


## Building Docker image

To build all the images, you can use the docker command `docker build`

    $ cd /var/docker/debian/8
    $ docker build -t GitLabCI:4567/docker/salt/debian/8 .
    ... take a 5 minutes rest ...

    $ docker images
    REPOSITORY                                               TAG                 IMAGE ID            CREATED             SIZE
    GitLabCI:4567/docker/salt/debian/8   latest              5dda22668520        2 hours ago         525MB
    GitLabCI:4567/docker/salt/centos/7   latest              fb493842efab        4 hours ago         381MB
    ...

## Usage & Testing

    $ docker run -d --privileged -v /sys/fs/cgroup:/sys/fs/cgroup:ro GitLabCI:4567/docker/salt/debian/8

    $ docker exec -it $CONTAINER_ID bash


## Minion's configuration

To update the masterless Minion's configuration, edit this and rebuild the image(s):

* CentOS 7 ([`Config`](./centos/7/conf/minion.d/))
* Debian 8 ([`Config`](./debian/8/conf/minion.d/))

## References

* [docker-salt-masterless](https://github.com/psmiraglia/docker-salt-masterless)
* [jrei/systemd-debian](https://hub.docker.com/r/jrei/systemd-debian/)
* [centos/systemd](https://hub.docker.com/r/centos/systemd/)
* [Salt Masterless Quickstart](https://docs.saltstack.com/en/latest/topics/tutorials/quickstart.html)
* [Configuring the Salt Minion](https://docs.saltstack.com/en/latest/ref/configuration/minion.html)
* [Configuring the Salt Master](https://docs.saltstack.com/en/latest/ref/configuration/master.html)
* [Install Docker Engine](https://docs.docker.com/engine/installation)
* [Build an image from a `Dockerfile`](https://docs.docker.com/engine/userguide/containers/dockerimages/#building-an-image-from-a-dockerfile)
