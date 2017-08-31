FROM docker:17.06.1-ce as static-docker-source
# the `docker` repository on dockerhub provides a source of prebuild static binaries.
# These may conveniently be referenced in a multi-stage build to copy over the
# static docker binary without taking on any additional files.

FROM alpine:3.5
ENV CLOUD_SDK_VERSION 168.0.0

ENV PATH /google-cloud-sdk/bin:$PATH
RUN apk --no-cache add \
        curl \
        python \
        py-crcmod \
        bash \
        libc6-compat \
        openssh-client \
        git \
    && curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-${CLOUD_SDK_VERSION}-linux-x86_64.tar.gz && \
    tar xzf google-cloud-sdk-${CLOUD_SDK_VERSION}-linux-x86_64.tar.gz && \
    rm google-cloud-sdk-${CLOUD_SDK_VERSION}-linux-x86_64.tar.gz && \
    ln -s /lib /lib64 && \
    gcloud config set core/disable_usage_reporting true && \
    gcloud config set component_manager/disable_update_check true && \
    gcloud config set metrics/environment github_docker_image
COPY --from=static-docker-source /usr/local/bin/docker /usr/local/bin/docker
VOLUME ["/root/.config"]
