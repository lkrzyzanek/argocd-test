FROM alpine:latest

RUN apk --update --no-cache add bash rsync && \
    rm -rf /var/cache/apk/*

ADD /content /content

