FROM redhat/ubi8:8.10

RUN yum install -y git iputils wget jq \
    && yum autoremove -y \
    && yum clean all \
    && rm -rf /var/cache/yum
