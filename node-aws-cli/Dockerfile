FROM amazonlinux:2 as installer

RUN yum update -y \
  && yum install -y unzip \
  && curl https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip -o awscli-exe-linux-x86_64.zip \
  && unzip awscli-exe-linux-x86_64.zip \
  && ./aws/install --bin-dir /aws-cli-bin/

FROM amazonlinux:2
COPY --from=installer /usr/local/aws-cli/ /usr/local/aws-cli/
COPY --from=installer /aws-cli-bin/ /usr/local/bin/

RUN yum update -y \
  && yum install -y less groff jq \
  && yum clean all


# Install C and wget
RUN yum install -y gcc-c++ make

# Install node
RUN curl -sL https://rpm.nodesource.com/setup_12.x | bash -

RUN yum install -y nodejs
