FROM ubuntu:24.04

LABEL maintainer="karolisvaitiekunas@protonmail.com"
LABEL description="Jellyfin docker image"

ENV JELLYFIN_HOME=/opt/jellyfin
ENV JELLYFIN_FFMPEG_HOME=/usr/share/jellyfin-ffmpeg

ARG JELLY_VERSION
ARG JELLY_BRANCH
ARG JFFMPEG_VERSION

# Install necessary packages
RUN apt-get update && apt-get install -y \
    ffmpeg \
    xz-utils \
    curl

RUN mkdir -p $JELLYFIN_HOME $JELLYFIN_FFMPEG_HOME /home/Jellyfin/downloads

# Download jellyfin
RUN curl -fsSL "https://repo.jellyfin.org/files/server/linux/${JELLY_BRANCH}/v${JELLY_VERSION}/amd64/jellyfin_${JELLY_VERSION}-amd64.tar.gz" \
    | tar xzf - -C $JELLYFIN_HOME --strip-components=1

# Downlad  jellyfin-ffmpeg
RUN curl -fsSL  "https://repo.jellyfin.org/files/ffmpeg/linux/7.x/${JFFMPEG_VERSION}/amd64/jellyfin-ffmpeg_${JFFMPEG_VERSION}_portable_linux64-gpl.tar.xz" \
        | tar xJf - -C $JELLYFIN_FFMPEG_HOME


HEALTHCHECK --interval=30s --timeout=5s --retries=2 CMD curl -f http://localhost:8096 || exit 1

WORKDIR $JELLYFIN_HOME

EXPOSE 8096

# Start Jellyfin
CMD ["./jellyfin", "-d", "/opt/jellyfin/data", "-C", "/opt/jellyfin/cache", "-c", "/opt/jellyfin/config", "-l", "/opt/jellyfin/log", "--ffmpeg", "/usr/share/jellyfin-ffmpeg/ffmpeg"]
