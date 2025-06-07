ARG ARGOCD_VERSION=v3.0.5

FROM quay.io/argoproj/argocd:${ARGOCD_VERSION}

ENV KUBECTL_VERSION=1.30.1

USER root

# Install requirements
RUN apt-get update \
    && apt-get install --no-install-recommends -y curl gnupg

# Install kubectl
RUN curl -LO https://dl.k8s.io/release/v${KUBECTL_VERSION}/bin/linux/amd64/kubectl \
    && mv kubectl /usr/local/bin/kubectl \
    && chmod +x /usr/local/bin/kubectl

# Cleanup
RUN apt-get autoremove -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

USER $ARGOCD_USER_ID
