FROM alpine:latest

RUN set -x \
    && apk add python \
               py-pip \
    && pip install anchorecli

ENV ANCHORE_CLI_URL="http://anchore-engine:8223/v1" \
    ANCHORE_CLI_USER=admin \
    ANCHORE_CLI_PASS="foobar"

CMD ["anchore-cli", "--help" ]


