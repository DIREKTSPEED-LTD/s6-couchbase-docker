FROM ubuntu:12.04

MAINTAINER Couchbase Docker Team <docker@couchbase.com>

# Install dependencies
RUN apt-get update && \
    apt-get install -yq runit wget python-httplib2  && \
    apt-get autoremove && apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV CB_VERSION=4.0.0-rc0 \
    CB_RELEASE_URL=http://packages.couchbase.com/releases \
    CB_PACKAGE=couchbase-server-enterprise_4.0.0-rc0-ubuntu12.04_amd64.deb \
    CB_SHA256=7c37660632dd5da27c75df451da54bc48ed721d561a5a8063b9c0a7f9220342a \
    PATH=$PATH:/opt/couchbase/bin:/opt/couchbase/bin/tools:/opt/couchbase/bin/install

# Install couchbase
RUN wget -N $CB_RELEASE_URL/$CB_VERSION/$CB_PACKAGE && \
    echo "$CB_SHA256  $CB_PACKAGE" | sha256sum -c - && \
    dpkg -i ./$CB_PACKAGE && rm -f ./$CB_PACKAGE

# Add runit script for couchbase-server
COPY scripts/run /etc/service/couchbase-server/run
RUN chmod 755 /etc/service/couchbase-server/run

# Add bootstrap script
COPY scripts/entrypoint.sh /
ENTRYPOINT ["/entrypoint.sh"]
CMD ["couchbase-server"]

EXPOSE 8091 8092 11207 11210 11211 18091 18092
VOLUME /opt/couchbase/var
