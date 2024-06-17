ARG JNLP_VERSION=3248.v65ecb_254c298-5
ARG TERRAFORM_VERSION=1.8.5

FROM hashicorp/terraform:${TERRAFORM_VERSION} AS terraform-builder

FROM jenkins/inbound-agent:${JNLP_VERSION} AS jnlp

USER root

RUN curl -fsSL https://deb.nodesource.com/setup_20.x -o /tmp/nodesource_setup.sh \
    && chmod a+x /tmp/nodesource_setup.sh \
    && /tmp/nodesource_setup.sh

RUN apt-get update \
    && apt-get install --no-install-recommends -y \
    wget jq yq nodejs make \
    && apt-get autoremove -y && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY --from=terraform-builder /bin/terraform /bin/

USER jenkins
