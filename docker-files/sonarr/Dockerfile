FROM alpine:3.20.3

LABEL maintainer="karolisvaitiekunas@protonmail.com"
LABEL description="Sonarr docker image"

ENV SONARR_HOME=/opt/Sonarr
ENV SONARR_VAR_LIB=/var/lib/sonarr
ENV SONARR_CONFIG=/.config/Sonarr

ARG VERSION
ARG BRANCH

RUN apk add --no-cache \
        icu-libs \
        sqlite-libs \
        xmlstarlet \
        curl

RUN mkdir -p $SONARR_HOME $SONARR_VAR_LIB /download/tv-shows

RUN curl -fsSL "https://services.sonarr.tv/v1/update/${BRANCH}/download?version=${VERSION}&os=linuxmusl&runtime=netcore&arch=x64" \
        | tar xzf - -C $SONARR_HOME --strip-components=1

WORKDIR $SONARR_HOME

HEALTHCHECK --interval=30s --timeout=5s --retries=2 CMD curl -f http://localhost:8989 || exit 1

EXPOSE 8989

# Start Sonarr
CMD ["./Sonarr", "--nobrowser", "--data=/.config/Sonarr"]
