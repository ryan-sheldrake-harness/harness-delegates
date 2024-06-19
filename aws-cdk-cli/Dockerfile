# Copyright 2022 Harness Inc. All rights reserved.
# Use of this source code is governed by the PolyForm Free Trial 1.0.0 license
# that can be found in the licenses directory at the root of this repository, also available at
# https://polyformproject.org/wp-content/uploads/2020/05/PolyForm-Free-Trial-1.0.0.txt.

FROM ubuntu:20.04

LABEL name="harness/delegate" \
      vendor="Harness" \
      maintainer="Harness"

RUN apt-get update  \
  && apt-get install -y \
    python3 \
    procps \
    hostname \
    lsof \
    findutils \
    tar \
    gzip \
    zip \
    git \
    locales \
    curl \
  && useradd -u 1001 -g 0 harness \
  && apt-get clean all \
  && locale-gen en_US.UTF-8 \
  && mkdir -p /opt/harness-delegate/

COPY immutable-scripts /opt/harness-delegate/

WORKDIR /opt/harness-delegate

RUN mkdir -p /opt/harness-delegate/additional_certs_pem_split

COPY --from=eclipse-temurin:17.0.7_7-jre /opt/java/openjdk/ /opt/java/openjdk/

ENV LANG=en_US.UTF-8
ENV HOME=/opt/harness-delegate
ENV CLIENT_TOOLS_DOWNLOAD_DISABLED=true
ENV INSTALL_CLIENT_TOOLS_IN_BACKGROUND=true
ENV JAVA_HOME=/opt/java/openjdk/
ENV PATH="$JAVA_HOME/bin:${PATH}"

ARG TARGETARCH
ARG BASEURL=https://app.harness.io/public/shared/delegates
ARG DELEGATEVERSION

# Note for amd 64 build use #   && curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" \
RUN mkdir -m 777 -p client-tools/aws-cli \
  && cd client-tools/aws-cli \
  && curl "https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip" -o "awscliv2.zip"  \
  && unzip awscliv2.zip \
  && ./aws/install 

RUN mkdir -m 777 -p client-tools/harness-credentials-plugin/v0.1.0 \
  && curl -s -L -o client-tools/harness-credentials-plugin/v0.1.0/harness-credentials-plugin https://app.harness.io/public/shared/tools/harness-credentials-plugin/release/v0.1.0/bin/linux/$TARGETARCH/harness-credentials-plugin \
  && chmod -R 775 /opt/harness-delegate \
  && chgrp -R 0 /opt/harness-delegate  \
  && chown -R 1001 /opt/harness-delegate \
  && chown -R 1001 $JAVA_HOME/lib/security/cacerts


ENV PATH=/opt/harness-delegate/client-tools/harness-credentials-plugin/v0.1.0/:$PATH
ENV SHARED_CA_CERTS_PATH=/opt/harness-delegate/additional_certs_pem_split
ENV PATH=/opt/harness-delegate/client-tools/aws-cli/:$PATH

COPY delegate.jar /opt/harness-delegate/delegate.jar
# RUN curl -s -L -o delegate.jar $BASEURL/$DELEGATEVERSION/delegate.jar

USER 1001

CMD [ "./start.sh" ]
