# syntax = docker.io/docker/dockerfile:1.1.3-experimental

# https://github.com/GoogleContainerTools/container-debug-support/blob/duct-tape/go/Dockerfile

# target: duct-tape-go-builder
ARG GOLANG_VERSION
FROM docker.io/golang:${GOLANG_VERSION}-buster AS duct-tape-go-builder
ARG DELVE_VERSION
ENV OUTDIR='/out' \
	GO111MODULE='on'
RUN set -eux && \
	wget -q -O delve-${DELVE_VERSION}.tar.gz https://github.com/go-delve/delve/archive/v${DELVE_VERSION}.tar.gz && \
	tar xzf delve-${DELVE_VERSION}.tar.gz && \
	cd delve-${DELVE_VERSION}/cmd/dlv && \
	GOBIN=${OUTDIR} go install -a -v -installsuffix='cgo' -ldflags='-s -w "-extldflags=-fno-PIC -static"' .

# target: duct-tape-go
FROM gcr.io/distroless/base:debug-nonroot AS duct-tape-go
WORKDIR /duct-tape
COPY --from=duct-tape-go-builder /out/dlv go/bin/
CMD ["/bin/sh", "/install.sh"]
