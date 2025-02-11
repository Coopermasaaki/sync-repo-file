FROM openeuler/openeuler:23.03 as BUILDER
RUN dnf update -y && \
    dnf install -y golang && \
    go env -w GOPROXY=https://goproxy.cn,direct

MAINTAINER zengchen1024<chenzeng765@gmail.com>

# build binary
COPY . /go/src/github.com/opensourceways/sync-repo-file
RUN cd /go/src/github.com/opensourceways/sync-repo-file && CGO_ENABLED=1 go build -v -o ./sync-repo-file ./do-once

# copy binary config and utils
FROM openeuler/openeuler:22.03
RUN dnf -y update && \
    dnf in -y shadow && \
    groupadd -g 1000 sync-repo-file && \
    useradd -u 1000 -g sync-repo-file -s /bin/bash -m sync-repo-file && \
    mkdir -p /opt/app/

USER sync-repo-file

# overwrite config yaml
COPY  --chown=sync-repo-file --from=BUILDER /go/src/github.com/opensourceways/sync-repo-file/sync-repo-file /opt/app

WORKDIR /opt/app/
ENTRYPOINT ["/opt/app/sync-repo-file"]
