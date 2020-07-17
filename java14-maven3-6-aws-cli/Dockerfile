FROM amazonlinux:2 as installer

RUN yum update -y \
  && yum install -y unzip tar gzip \
  && curl https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip -o awscli-exe-linux-x86_64.zip \
  && unzip awscli-exe-linux-x86_64.zip \
  && ./aws/install --bin-dir /aws-cli-bin/

FROM maven:3.6.3-openjdk-14
COPY --from=installer /usr/local/aws-cli/ /usr/local/aws-cli/
COPY --from=installer /aws-cli-bin/ /usr/local/bin/

RUN ["aws", "--version"]
RUN ["java", "-version"]
RUN ["mvn", "-version"]

CMD ["bash"]