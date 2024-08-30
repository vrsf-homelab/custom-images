ARG GHA_RUNNER_VERSION=2.319.1
ARG TERRAFORM_VERSION=1.8.5
ARG YQ_VERSION=4.44.3

FROM hashicorp/terraform:${TERRAFORM_VERSION} AS terraform-builder
FROM mikefarah/yq:${YQ_VERSION} AS yq-builder
FROM ghcr.io/actions/actions-runner:${GHA_RUNNER_VERSION} AS actions-runner

USER root

RUN apt-get update \
    && apt-get install --no-install-recommends -y \
    curl wget jq make git

RUN curl -fsSL https://deb.nodesource.com/setup_20.x -o /tmp/nodesource_setup.sh \
    && chmod a+x /tmp/nodesource_setup.sh \
    && /tmp/nodesource_setup.sh

RUN apt-get update \
    && apt-get install --no-install-recommends -y \
    nodejs \
    && apt-get autoremove -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY --from=terraform-builder /bin/terraform /bin/
COPY --from=yq-builder /usr/bin/yq /bin/

USER runner
