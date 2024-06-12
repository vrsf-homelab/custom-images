ARG ARGOCD_VERSION=v2.11.3
ARG KSOPS_VERSION=v4.3.1

FROM quay.io/viaductoss/ksops:${KSOPS_VERSION} as ksops-builder

FROM quay.io/argoproj/argocd:${ARGOCD_VERSION}

ENV SOPS_VERSION=3.8.1 \
    KUBECTL_VERSION=1.30.1

USER root

# Install requirements
RUN apt-get update \
    && apt-get install --no-install-recommends -y curl gnupg

# Install SOPS
RUN curl -LO https://github.com/getsops/sops/releases/download/v${SOPS_VERSION}/sops-v${SOPS_VERSION}.linux.amd64 \
    && mv sops-v${SOPS_VERSION}.linux.amd64 /usr/local/bin/sops \
    && chmod +x /usr/local/bin/sops

# Install kubectl
RUN curl -LO https://dl.k8s.io/release/v${KUBECTL_VERSION}/bin/linux/amd64/kubectl \
    && mv kubectl /usr/local/bin/kubectl \
    && chmod +x /usr/local/bin/kubectl

# Override the default kustomize executable with the Go built version
COPY --from=ksops-builder /usr/local/bin/kustomize /usr/local/bin/kustomize

# Add ksops executable to path
COPY --from=ksops-builder /usr/local/bin/ksops /usr/local/bin/ksops

# Cleanup
RUN apt-get autoremove -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

USER $ARGOCD_USER_ID
