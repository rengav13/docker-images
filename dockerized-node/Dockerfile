FROM amazonlinux:2 as installer

RUN yum update -y \
  && yum install -y unzip \
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


FROM amazonlinux:2
COPY --from=installer /usr/local/aws-cli/ /usr/local/aws-cli/
COPY --from=installer /aws-cli-bin/ /usr/local/bin/
COPY --from=docker-installer /usr/local/bin/docker /usr/local/bin/

RUN yum update -y \
  && yum install -y less groff jq \
  && yum clean all


# Install C and wget
RUN yum install -y gcc-c++ make

# Install node
RUN curl -sL https://rpm.nodesource.com/setup_12.x | bash -

RUN yum install -y nodejs
