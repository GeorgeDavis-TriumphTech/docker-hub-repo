ARG ARG_BASE_VERSION=1.3.7
FROM hashicorp/terraform:$ARG_BASE_VERSION

# ARG_AWSCLI_VERSION has to be a specific version number, available on https://awscli.amazonaws.com/ and be a tar.gz file. Eg. 2.11.15
ARG ARG_AWSCLI_VERSION=2.11.15

ENV AWSCLI_VERSION=$ARG_AWSCLI_VERSION \
    PYTHONUNBUFFERED=1 \ 
    AWS_ACCESS_KEY_ID=AKIA2HWNBEWP6CF762QC \
    AWS_SECRET_ACCESS_KEY=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 \
    AWS_DEFAULT_REGION=${AWS_DEFAULT_REGION} \
    AWS_REGION=${AWS_DEFAULT_REGION}

# Building aws-cli from source, to run on alpinelinux (hashicorp/terraform Docker container)
# Source: AWS Docs - https://docs.aws.amazon.com/cli/latest/userguide/getting-started-source-install.html#source-getting-started-install-workflows-alpine
RUN apk add --no-cache --quiet --no-progress \
    python3-dev \
    python3 \
    curl \
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

CMD [ "terraform" ]