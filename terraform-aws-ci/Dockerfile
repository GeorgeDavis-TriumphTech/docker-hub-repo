ARG ARG_BASE_VERSION=latest
FROM alpine:$ARG_BASE_VERSION

# ARG_AWSCLI_VERSION has to be a specific version number, available on https://awscli.amazonaws.com/ and be a tar.gz file. Eg. 2.11.15
ARG ARG_AWSCLI_VERSION=2.11.15
ARG ARG_TERRAFORM_VERSION=1.3.7

ENV AWSCLI_VERSION=$ARG_AWSCLI_VERSION \
    TERRAFORM_VERSION=${ARG_TERRAFORM_VERSION} \
    PYTHONUNBUFFERED=1 \ 
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

RUN wget https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip && \
    unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip && \
    mv terraform /usr/bin/terraform

RUN rm -rf awscli-${AWSCLI_VERSION} && \
    rm -rf terraform_${TERRAFORM_VERSION}_linux_amd64.zip

CMD [ "/usr/bin/terraform" ]