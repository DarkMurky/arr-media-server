FROM alpine:3.20.3

LABEL maintainer="karolisvaitiekunas@protonmail.com"
LABEL description="Radarr docker image"

ENV RADARR_HOME=/opt/Radarr
ENV RADARR_VAR_LIB=/var/lib/radarr
ENV RADARR_CONFIG=/.config/Radarr

ARG VERSION
ARG BRANCH

RUN apk add --no-cache \
        icu-libs \
        sqlite-libs \
        xmlstarlet \
        curl

RUN mkdir -p $RADARR_HOME $RADARR_VAR_LIB /download/movies

# Download and extract Radarr
RUN curl -fsSL "https://radarr.servarr.com/v1/update/${BRANCH}/updatefile?version=${VERSION}&os=linuxmusl&runtime=netcore&arch=x64" \
        | tar xzf - -C $RADARR_HOME --strip-components=1

WORKDIR $RADARR_HOME

HEALTHCHECK --interval=30s --timeout=5s --retries=2 CMD curl -f http://localhost:7878 || exit 1

EXPOSE 7878

# Start Radarr
CMD ["./Radarr", "--nobrowser", "--data=/.config/Radarr"]
