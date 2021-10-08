# Build the mattermost cloud
ARG DOCKER_BUILD_IMAGE=golang:1.17.0
ARG DOCKER_BASE_IMAGE=alpine:3.11.3

FROM ${DOCKER_BUILD_IMAGE} AS build
WORKDIR /cloud-db-factory-horizontal-scaling/
COPY . /cloud-db-factory-horizontal-scaling/
RUN apt-get update -yq && apt-get install -yq unzip
RUN make build


# Final Image
FROM ${DOCKER_BASE_IMAGE}
LABEL name="Mattermost Cloud Database Factory Horizontal Scaling" \
  maintainer="cloud-team@mattermost.com" \
  vendor="Mattermost" \
  distribution-scope="public" \
  architecture="x86_64" \
  url="https://mattermost.com" \
  io.k8s.description="Mattermost Cloud Database Factory Horizontal Scaling is responsible for RDS cluster scaling" \
  io.k8s.display-name="Mattermost Cloud Database Factory"

ENV CLOUD=/cloud-db-factory-horizontal-scaling/main \
    USER_UID=10001 \
    USER_NAME=cloud

RUN  apk update && apk add libc6-compat && apk add ca-certificates
COPY --from=build /cloud-db-factory-horizontal-scaling/build/_output/bin/main /cloud-db-factory-horizontal-scaling/main
COPY --from=build /cloud-db-factory-horizontal-scaling/build/bin /usr/local/bin

RUN  /usr/local/bin/user_setup
WORKDIR /cloud-db-factory-horizontal-scaling/

ENTRYPOINT ["/usr/local/bin/entrypoint"]

USER ${USER_UID}
