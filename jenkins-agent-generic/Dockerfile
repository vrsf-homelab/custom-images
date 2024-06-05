ARG AGENT_VERSION=bookworm-jdk17
ARG TERRAFORM_VERSION=1.8.5

FROM hashicorp/terraform:${TERRAFORM_VERSION} AS terraform-builder

FROM jenkins/agent:${AGENT_VERSION} AS jnlp

USER root

RUN curl -fsSL https://deb.nodesource.com/setup_20.x -o /tmp/nodesource_setup.sh \
    && chmod a+x /tmp/nodesource_setup.sh \
    && /tmp/nodesource_setup.sh

RUN apt-get update \
    && apt-get install --no-install-recommends -y \
    wget jq yq nodejs \
    && apt-get autoremove -y && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY --from=terraform-builder /bin/terraform /bin/

USER jenkins
