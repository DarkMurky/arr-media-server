FROM alpine:3.20.3

LABEL maintainer="karolisvaitiekunas@protonmail.com"
LABEL description="jellyseerr docker image"

ENV JELLY_HOME=/opt/jellyseerr
ENV JELLY_CONFIG=/etc/jellyseerr/jellyseerr.conf

RUN apk add --no-cache  \
		nodejs \
		git \
		curl && \
		curl -L https://unpkg.com/@pnpm/self-installer | node

RUN mkdir -p $JELLY_HOME

WORKDIR $JELLY_HOME

# Download and build jellyseerr
RUN git clone https://github.com/Fallenbagel/jellyseerr.git && \
	cd jellyseerr  && \
	CYPRESS_INSTALL_BINARY=0 pnpm install --frozen-lockfile && \
	pnpm build && \
	rm -rf jellyseerr/.git

WORKDIR $JELLY_HOME/jellyseerr

HEALTHCHECK --interval=30s --timeout=5s --retries=2 CMD curl -f http://localhost:5055 || exit 1

EXPOSE 5055

# Start jellyseerr
CMD [ "pnpm", "start" ]
