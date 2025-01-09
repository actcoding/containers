ARG PUSH_VERSION=0.7.0

FROM rust:1-alpine AS build

ADD https://github.com/nextcloud/notify_push#v${PUSH_VERSION} /usr/local/src

WORKDIR /usr/local/src

RUN apk add --no-cache \
        musl-dev \
    && \
    cargo build --release


FROM alpine:3

COPY --from=build /usr/local/src/result/bin/notify_push /usr/local/bin/notify_push

CMD [ "/usr/local/bin/notify_push", "--no-ansi", "--port", "1337", "--bind", "0.0.0.0" ]
