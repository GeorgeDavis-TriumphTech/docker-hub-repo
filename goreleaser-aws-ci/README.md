# goreleaser-aws-ci Docker Build

This image provides the use of GoReleaser and AWS CLI in one simple lightweight build image, rather than installing these utilities at build time, reducing build times and optimizing build CI minutes/costs.

It exposes `goreleaser` as an executable.

Source: [https://github.com/GeorgeDavis-TriumphTech/docker-hub-repo/tree/main/goreleaser-aws-ci](https://github.com/GeorgeDavis-TriumphTech/docker-hub-repo/tree/main/goreleaser-aws-ci)

## Usage 

```sh
docker run \
--platform linux/amd64 \
-e AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} \
-e AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} \
-e AWS_DEFAULT_REGION=${AWS_DEFAULT_REGION} \
goreleaser-aws-ci:latest <your-aws-or-goreleaser-command>
```

| Environment Variables  | Default  | Required  |
| ------------- | ------------- | ------------- |
| AWS_ACCESS_KEY_ID  | ${AWS_ACCESS_KEY_ID}  | Required  |
| AWS_SECRET_ACCESS_KEY  | ${AWS_SECRET_ACCESS_KEY}  | Required  |
| AWS_DEFAULT_REGION  | ${AWS_DEFAULT_REGION}  | Required  |
| AWS_REGION  | ${AWS_DEFAULT_REGION}  | N/A  |

### Sample Bitbucket Pipeline step

```
- step:
    name: Deploy to Production
    deployment: Production
    trigger: manual    
    script:
        - docker pull georgedtriumphtech/goreleaser-aws-ci:latest
        - |-
            docker run \
            --platform linux/amd64 \
            -v $BITBUCKET_CLONE_DIR:/ \
            -e AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} \
            -e AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} \
            -e AWS_DEFAULT_REGION=${AWS_DEFAULT_REGION} \
            goreleaser-aws-ci:latest 
    caches:
        - docker
    service:
        - docker
    artifacts:
        - dist/**
```

## Docker Build

| Build Variables  | Default |
| ------------- | ------------- |
| ARG_BASE_VERSION  | latest  |
| ARG_AWSCLI_VERSION  | 2.11.15  |


You can override any of these build arguments with `--build-arg ARG_BASE_VERSION=`

Example:
```sh
DOCKER_BUILDKIT=1 \
docker build \
--platform linux/amd64 \
--build-arg ARG_AWSCLI_VERSION=<ARG_AWSCLI_VERSION> \
-t goreleaser-aws-ci .
```

### Credits

This image was built using GitHub Actions. Kudos to GitHub for the free CI/CD minutes every month for making these projects happen.