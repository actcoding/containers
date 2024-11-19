ARG KEYCLOAK_VERSION=26.0.5

FROM alpine:3 as theme

RUN apk add --no-cache git && \
    git clone --branch act -- https://github.com/actcoding/keywind.git /keywind


FROM quay.io/keycloak/keycloak:${KEYCLOAK_VERSION} as builder

ENV KC_HEALTH_ENABLED=true
ENV KC_METRICS_ENABLED=true
ENV KC_FEATURES=token-exchange,recovery-codes,web-authn,passkeys
ENV KC_DB=postgres

RUN /opt/keycloak/bin/kc.sh build


FROM quay.io/keycloak/keycloak:${KEYCLOAK_VERSION}

COPY --from=builder /opt/keycloak/ /opt/keycloak/

WORKDIR /opt/keycloak

# COPY ./keywind/theme/keywind themes/keywind
COPY --from=theme /keywind/theme/keywind themes/keywind

# ENTRYPOINT [ "/opt/keycloak/bin/kc.sh", "start-dev" ]
ENTRYPOINT [ "/opt/keycloak/bin/kc.sh", "start", "--optimized", "--proxy=edge" ]
