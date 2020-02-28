ARG BUILD_FROM
FROM elixir:1.10-alpine AS builder

RUN apk add --update --no-cache nodejs npm git build-base && \
    mix local.rebar --force && \
    mix local.hex --force

ENV MIX_ENV=prod

WORKDIR /opt/app

COPY mix.exs mix.lock ./
RUN mix "do" deps.get --only $MIX_ENV, deps.compile

COPY assets assets
RUN npm ci --prefix ./assets && npm run deploy --prefix ./assets

COPY config config
COPY lib lib
COPY priv priv
COPY grafana/dashboards grafana/dashboards

RUN mix "do" phx.digest, compile

RUN mkdir -p /opt/built && mix release --path /opt/built

########################################################################

FROM $BUILD_FROM as app

##FROM alpine:3.11 AS app

ENV LANG=C.UTF-8 \
    SRTM_CACHE=/opt/app/.srtm_cache \
    HOME=/opt/app

RUN apk add --update --no-cache bash openssl tzdata

WORKDIR $HOME
RUN chown -R nobody: .
USER nobody

COPY --chown=nobody entrypoint.sh /
COPY --chown=nobody ha-bootstrap.sh /
COPY --from=builder --chown=nobody /opt/built .
RUN mkdir .srtm_cache

EXPOSE 8099

ENTRYPOINT ["/ha-bootstrap.sh"]
CMD ["bin/teslamate", "start"]