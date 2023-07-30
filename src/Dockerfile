ARG TAILSCALE_VERSION=v1.44

FROM tailscale/tailscale:$TAILSCALE_VERSION
# inspired by the work in https://github.com/hollie/tailscale-caddy-proxy
LABEL maintainer="falkner.dominik@gmail.com"

# region required config
# The target of the reverse proxy. Can be any service / host reachable from the container with port (e.g. nginx:80)
ENV CADDY_TARGET=
# Your tailnet
ENV TS_TAILNET=
# Machine name on the tailscale network excluding the .ts.net suffix (tailnetNNNN)
ENV TS_HOSTNAME=
# The ephemeral auth key (generated in the tailscale admin console)
ENV TS_AUTHKEY=
# endregion

# Run in userspace
#- TS_USERSPACE=userspace-networking
ENV TS_USERSPACE=true
# State dir is set to mem: by default - everytime the container restarts it will connect from a blank state. 
# Use this container only with ephemeral keys
ENV TS_STATE_DIR=mem: 

RUN apk add --no-cache ca-certificates mailcap caddy

# Ensure Caddy can access the tailscale socket, Caddy expects it to be under /var/run/tailscale so make a symlink
RUN mkdir --parents /var/run/tailscale && ln -s /tmp/tailscaled.sock /var/run/tailscale/tailscaled.sock

# Add image version to file for traceablity
RUN echo "$TAILSCALE_VERSION" > /VERSION

# Add the modified startup script
COPY --chmod=755 start.sh /usr/bin/start.sh
CMD  [ "start.sh" ]
