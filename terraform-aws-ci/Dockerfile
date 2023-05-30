# Credits: https://github.com/zenika-open-source/terraform-aws-cli/blob/master/Dockerfile
# Setup build arguments
ARG ARG_AWSCLI_VERSION=2.11.15
ARG ARG_TERRAFORM_VERSION=1.4.0
ARG PYTHON_MAJOR_VERSION=3.9
ARG DEBIAN_VERSION=bullseye-20230109-slim
ARG DEBIAN_FRONTEND=noninteractive

# Download Terraform binary
FROM debian:${DEBIAN_VERSION} as terraform
ARG TARGETARCH=amd64
ARG ARG_TERRAFORM_VERSION=1.4.0
RUN apt-get update && apt-get install --no-install-recommends -y \
  libcurl4 \
  curl \
  ca-certificates=20210119 \
  unzip=6.0-26+deb11u1 \
  gnupg=2.2.27-2+deb11u2
WORKDIR /workspace
RUN curl --silent --show-error --fail --remote-name https://releases.hashicorp.com/terraform/${ARG_TERRAFORM_VERSION}/terraform_${ARG_TERRAFORM_VERSION}_linux_${TARGETARCH}.zip
RUN unzip -j terraform_${ARG_TERRAFORM_VERSION}_linux_${TARGETARCH}.zip

# Install AWS CLI using PIP
FROM debian:${DEBIAN_VERSION} as aws-cli
ARG ARG_AWSCLI_VERSION=2.11.15
ARG PYTHON_MAJOR_VERSION
RUN apt-get update && apt-get install -y --no-install-recommends \
    python3=${PYTHON_MAJOR_VERSION}.2-3 \
    python3-pip=20.3.4-4+deb11u1 \
  && pip3 install --no-cache-dir \
    setuptools==67.1.0 \
    awscli

# Build final image
FROM debian:${DEBIAN_VERSION} as build
LABEL maintainer="GeorgeDavis-TriumphTech@github"
ARG PYTHON_MAJOR_VERSION
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    ca-certificates=20210119\
    git \
    jq \
    python3=${PYTHON_MAJOR_VERSION}.2-3 \
    openssh-client=1:8.4p1-5+deb11u1 \
    postgresql-contrib \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* \
  && update-alternatives --install /usr/bin/python python /usr/bin/python${PYTHON_MAJOR_VERSION} 1
WORKDIR /workspace
COPY --from=terraform /workspace/terraform /usr/local/bin/terraform
COPY --from=aws-cli /usr/local/bin/aws* /usr/local/bin/
COPY --from=aws-cli /usr/local/lib/python${PYTHON_MAJOR_VERSION}/dist-packages /usr/local/lib/python${PYTHON_MAJOR_VERSION}/dist-packages
COPY --from=aws-cli /usr/lib/python3/dist-packages /usr/lib/python3/dist-packages

RUN groupadd --gid 1001 nonroot \
  # user needs a home folder to store aws credentials
  && useradd --gid nonroot --create-home --uid 1001 nonroot \
  && chown nonroot:nonroot /workspace
USER nonroot

CMD ["bash"]