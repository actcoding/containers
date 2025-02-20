ARG KEYCLOAK_VERSION=26.0.5

FROM alpine:3 AS theme

RUN apk add --no-cache \
      curl \
      jq \
      wget

WORKDIR /usr/local/src

RUN --mount=type=secret,id=gh_pat \
    curl -q -H "Authorization: Bearer $(cat /run/secrets/gh_pat)" \
      "https://api.github.com/repos/actcoding/keycloak-theme/actions/artifacts" |\
    jq -r '.artifacts[] | select(.name == "theme") | .archive_download_url' |\
    xargs curl -q -L -H "Authorization: Bearer $(cat /run/secrets/gh_pat)" --output theme.zip && \
    unzip theme.zip


FROM quay.io/keycloak/keycloak:${KEYCLOAK_VERSION} AS builder

ENV KC_HEALTH_ENABLED=true
ENV KC_METRICS_ENABLED=true
ENV KC_FEATURES=token-exchange,recovery-codes,web-authn,passkeys
ENV KC_FEATURES_DISABLED=ciba,par,kerberos,organization
ENV KC_DB=postgres

COPY --from=theme /usr/local/src/keycloak-theme-for-kc-all-other-versions.jar /opt/keycloak/providers

RUN /opt/keycloak/bin/kc.sh build


FROM quay.io/keycloak/keycloak:${KEYCLOAK_VERSION}

COPY --from=builder /opt/keycloak/ /opt/keycloak/

WORKDIR /opt/keycloak

ENTRYPOINT [ "/opt/keycloak/bin/kc.sh", "start", "--optimized", "--proxy=edge" ]
