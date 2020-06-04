
FROM openjdk:11.0.2-jre-slim-stretch

COPY besu /opt/besu/
WORKDIR /opt/besu

EXPOSE 8545 8546 8547 30303

# defaults for host interfaces
ENV BESU_RPC_HTTP_HOST 0.0.0.0
ENV BESU_RPC_WS_HOST 0.0.0.0
ENV BESU_GRAPHQL_HTTP_HOST 0.0.0.0
ENV BESU_PID_PATH "/tmp/pid"

ENV PATH="/opt/besu/bin:${PATH}"
HEALTHCHECK --start-period=5s --interval=5s --timeout=1s --retries=10 CMD bash -c "[ -f /tmp/pid ]"

# Copy new entrypoint scripts
COPY *_start.sh /opt/besu/

# Install a dos 2 unix EOL converter (supporting either alpine or ubuntu images)
RUN (apt-get update && apt-get install dos2unix || apk add --update dos2unix) && \
    rm -rf /var/cache/apt/* && rm -rf /var/cache/apk/*

# Run dos2unix EOL conversion on all shell scripts to prevent scripts to fail if edited with a windows IDE
# that rewrote the EOL to CRLF before we build the image. See issue #4
RUN find /opt/besu/*.sh -type f -print0 | xargs -0 dos2unix

# specify default entrypoint to start the node
ENTRYPOINT ["/opt/besu/node_start.sh"]
