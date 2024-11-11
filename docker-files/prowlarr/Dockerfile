FROM alpine:3.20.3

LABEL maintainer="karolisvaitiekunas@protonmail.com"
LABEL description="Prowlarr docker image"

ENV PROWLARR_HOME=/opt/prowlarr

ARG BRANCH
ARG VERSION

RUN apk add --no-cache \
        icu-libs \
        sqlite-libs \
        xmlstarlet \
        curl

RUN mkdir -p $PROWLARR_HOME

# Download and extract Prowlarr
RUN curl -fsSL "https://prowlarr.servarr.com/v1/update/${BRANCH}/updatefile?version=${VERSION}&os=linuxmusl&runtime=netcore&arch=x64" \
        | tar xzf - -C $PROWLARR_HOME --strip-components=1

WORKDIR $PROWLARR_HOME

HEALTHCHECK --interval=30s --timeout=5s --retries=2 CMD curl -f http://localhost:9696 || exit 1

EXPOSE 9696

# Start Prowlarr
CMD ["./Prowlarr", "-nobrowser", "--data=/.config/Prowlarr"] 
