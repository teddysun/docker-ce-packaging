# syntax=docker/dockerfile:1

ARG GO_IMAGE=golang:latest
ARG DISTRO=almalinux
ARG SUITE=10
ARG BUILD_IMAGE=${DISTRO}:${SUITE}

FROM ${GO_IMAGE} AS golang

FROM ${BUILD_IMAGE}
ENV GOPROXY=https://proxy.golang.org|direct
ENV GO111MODULE=off
ENV GOPATH=/go
ENV GOTOOLCHAIN=local
ENV PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
ENV AUTO_GOPATH=1
ENV DOCKER_BUILDTAGS=exclude_graphdriver_btrfs
ARG DISTRO
ARG SUITE
ENV DISTRO=${DISTRO}
ENV SUITE=${SUITE}

RUN dnf install -y rpm-build dnf-plugins-core epel-release
RUN dnf config-manager --set-enabled epel
RUN dnf config-manager --set-enabled crb

COPY --link SPECS /root/rpmbuild/SPECS
RUN dnf builddep -y /root/rpmbuild/SPECS/*.spec
COPY --link --from=golang /usr/local/go /usr/local/go
WORKDIR /root/rpmbuild
ENTRYPOINT ["/bin/rpmbuild"]
