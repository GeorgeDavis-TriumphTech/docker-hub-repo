ARG ARG_BASE_VERSION=alpine
FROM golang:$ARG_BASE_VERSION

# ARG_AWSCLI_VERSION has to be a specific version number, available on https://awscli.amazonaws.com/ and be a tar.gz file. Eg. 2.11.15
ARG ARG_AWSCLI_VERSION=2.11.15
ARG ARG_GORELEASER_VERSION=1.18.2

ENV AWSCLI_VERSION=${ARG_AWSCLI_VERSION} \
    GORELEASER_VERSION=${ARG_GORELEASER_VERSION} \ 
    AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} \
    AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} \
    AWS_DEFAULT_REGION=${AWS_DEFAULT_REGION} \
    AWS_REGION=${AWS_DEFAULT_REGION}

# Building aws-cli from source, to run on alpinelinux (hashicorp/terraform Docker container)
# Source: AWS Docs - https://docs.aws.amazon.com/cli/latest/userguide/getting-started-source-install.html#source-getting-started-install-workflows-alpine
RUN apk add --no-cache --quiet --no-progress \
    python3-dev \
    python3 \
    curl \
    wget \
    make \
    cmake \
    gcc \
    g++ \
    libc-dev \
    libffi-dev \
    openssl-dev \
    tar \
    && curl https://awscli.amazonaws.com/awscli-${AWSCLI_VERSION}.tar.gz | tar -xz \
    && cd awscli-${AWSCLI_VERSION} \
    && ./configure --prefix=/opt/aws-cli/ --with-download-deps \
    && make \
    && make install

ENV PATH=$PATH:/opt/aws-cli/lib/aws-cli/bin

RUN wget "https://github.com/goreleaser/goreleaser/releases/download/v${GORELEASER_VERSION}/goreleaser_${GORELEASER_VERSION}_x86_64.apk" && \
    apk add --allow-untrusted goreleaser_${GORELEASER_VERSION}_x86_64.apk && \
    rm goreleaser_${GORELEASER_VERSION}_x86_64.apk && \
    rm /var/cache/apk/*

CMD [ "goreleaser" ]