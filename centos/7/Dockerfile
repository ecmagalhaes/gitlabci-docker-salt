FROM centos:7
RUN yum clean all && \
      yum install -y yum install epel-release && \
      yum install -y yum install https://repo.saltstack.com/yum/redhat/salt-repo-latest-2.el7.noarch.rpm  && \
      yum clean expire-cache && \
      yum update -y && \
      yum install -y sudo git tmux vim salt-minion && \
      yum clean all

# add minion's configuration modules
ADD conf/minion.d/* /etc/salt/minion.d/
