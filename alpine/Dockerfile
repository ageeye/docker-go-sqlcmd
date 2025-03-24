# syntax=docker/dockerfile:1
ARG GOLANG_VERSION=1.24
FROM golang:$GOLANG_VERSION-alpine AS build

ARG GO_SQLCMD_VERSION=1.8.2
ARG GO_SQLCMD_SHA256=4e97bf4ea6b00a43469156bde3ee91a622deed543b02b9b5dae3fa5565f6fea5

ADD https://api.github.com/repos/microsoft/go-sqlcmd/tarball/v$GO_SQLCMD_VERSION /tmp/gosqlcmd.tar.gz

RUN echo "$GO_SQLCMD_SHA256  /tmp/gosqlcmd.tar.gz" > /tmp/gosqlcmd.sha256 \
    && sha256sum -w -c /tmp/gosqlcmd.sha256

WORKDIR /tmp/gosqlcmd/
RUN tar -xv --strip-components=1 -f /tmp/gosqlcmd.tar.gz -C /tmp/gosqlcmd/
RUN go build -o sqlcmd \
    -ldflags="-X main.version=$GO_SQLCMD_VERSION" \
    /tmp/gosqlcmd/cmd/modern

FROM alpine

COPY --from=build /tmp/gosqlcmd/sqlcmd /usr/local/bin/sqlcmd
COPY --from=build /tmp/gosqlcmd/LICENSE /usr/share/licenses/sqlcmd/LICENSE

ENTRYPOINT ["tail", "-f", "/dev/null"]
