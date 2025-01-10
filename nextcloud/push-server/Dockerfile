FROM alpine:3

ARG PLATFORM
ARG PUSH_VERSION=0.7.0

ADD https://github.com/nextcloud/notify_push/releases/download/v${PUSH_VERSION}/notify_push-${PLATFORM}-unknown-linux-musl /usr/local/bin/notify_push

USER 1000

CMD [ "/usr/local/bin/notify_push", "--no-ansi", "--port", "1337", "--bind", "0.0.0.0" ]
