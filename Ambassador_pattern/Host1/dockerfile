FROM    alpine:3.2
MAINTAINER  SvenDowideit@home.org.au

RUN apk update && \
    apk add socat && \
    rm -r /var/cache/

CMD env | grep _TCP= | sed 's/.*_PORT_\([0-9]*\)_TCP=tcp:\/\/\(.*\):\(.*\)/socat -t 100000000 TCP4-LISTEN:\1,fork,reuseaddr TCP4:\2:\3 \& wait/' | sh
