FROM rust:1-alpine AS build

ARG PUSH_VERSION=0.7.0

RUN apk add --no-cache git && \
    git clone -b v${PUSH_VERSION} https://github.com/nextcloud/notify_push /usr/local/src

WORKDIR /usr/local/src

RUN apk add --no-cache \
        musl-dev \
    && \
    cargo build --release


FROM alpine:3

COPY --from=build /usr/local/src/target/release/notify_push /usr/local/bin/notify_push

CMD [ "/usr/local/bin/notify_push", "--no-ansi", "--port", "1337", "--bind", "0.0.0.0" ]
