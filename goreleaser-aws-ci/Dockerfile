ARG ARG_BASE_VERSION=latest
FROM golang:$ARG_BASE_VERSION

ENV AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} \
    AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} \
    AWS_DEFAULT_REGION=${AWS_DEFAULT_REGION} \
    AWS_REGION=${AWS_DEFAULT_REGION}

# Source: AWS Docs - https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
RUN echo 'deb [trusted=yes] https://repo.goreleaser.com/apt/ /' | tee /etc/apt/sources.list.d/goreleaser.list && \
    apt-get update && \
    apt-get install goreleaser curl unzip build-essential -y && \ 
    rm -rf /var/lib/apt/lists/*

RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    ./aws/install

CMD [ "goreleaser" ]