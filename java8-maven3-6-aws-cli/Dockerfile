FROM amazonlinux:2 as aws-cli-installer
RUN yum update -y \
  && yum install -y unzip tar gzip \
  && curl https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip -o awscli-exe-linux-x86_64.zip \
  && unzip awscli-exe-linux-x86_64.zip \
  && ./aws/install --bin-dir /aws-cli-bin/

FROM alpine:3.7 as docker-installer
ARG DOCKER_CLI_VERSION="18.09.2"
ENV DOWNLOAD_URL="https://download.docker.com/linux/static/stable/x86_64/docker-$DOCKER_CLI_VERSION.tgz"
# install docker client
RUN apk --update add curl \
    && mkdir -p /tmp/download \
    && curl -L $DOWNLOAD_URL | tar -xz -C /tmp/download \
    && mv /tmp/download/docker/docker /usr/local/bin/ \
    && rm -rf /tmp/download \
    && apk del curl \
    && rm -rf /var/cache/apk/* \
    && apk add --no-cache git openssh
RUN ["docker",  "--version"]

FROM maven:3.6.3-openjdk-8

COPY --from=aws-cli-installer /usr/local/aws-cli/ /usr/local/aws-cli/
COPY --from=aws-cli-installer /aws-cli-bin/ /usr/local/bin/
COPY --from=docker-installer /usr/local/bin/docker /usr/local/bin/

RUN yum install -y git

RUN ["aws", "--version"]
RUN ["java", "-version"]
RUN ["mvn", "-version"]
RUN ["docker", "--version"]

CMD [ "sleep", "9999999" ]
