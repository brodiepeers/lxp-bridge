ARG BUILD_VERSION
ARG BUILD_FROM

FROM brodiepeers/lxp-bridge:${BUILD_VERSION} AS lxp-bridge

FROM ghcr.io/home-assistant/amd64-base:latest
COPY --from=lxp-bridge / /

RUN echo "$MIRROR/alpine/edge/community" >> /etc/apk/repositories
RUN apk update
RUN apk add yq

COPY run.sh /
RUN chmod a+x /run.sh

CMD [ "/run.sh" ]
