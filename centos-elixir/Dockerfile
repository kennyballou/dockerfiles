FROM centos:7
MAINTAINER kballou@devnulllabs.io

ENV LANG="en_US.UTF-8"
ENV OTP_VER="22.2.2"
ENV REBAR_VERSION="2.6.4"
ENV REBAR3_VERSION="3.13.0"
ENV ELIXIR_VERSION="1.9.4"

RUN set -xe \
    && yum -y groupinstall "Development Tools" \
    && yum -y install ncurses \
        ncurses-devel \
        unixODBC \
        unixODBC-devel \
        openssl-devel \
    && OTP_SRC_URL="https://github.com/erlang/otp/archive/OTP-$OTP_VER.tar.gz" \
    && OTP_SRC_SUM="92df7d22239b09f7580572305c862da1fb030a97cef7631ba060ac51fa3864cc" \
    && curl -fSL "$OTP_SRC_URL" -o otp-src.tar.gz \
    && echo "${OTP_SRC_SUM}  otp-src.tar.gz" | sha256sum -c - \
    && mkdir -p /usr/src/otp-src \
    && tar -zxf otp-src.tar.gz -C /usr/src/otp-src --strip-components=1 \
    && rm otp-src.tar.gz \
    && cd /usr/src/otp-src \
    && ./otp_build autoconf \
    && ./configure \
    && make \
    && make install \
    && find /usr/local -name examples | xargs rm -rf \
    && cd /usr/src \
    && rm -rf /usr/src/otp-src \
    && REBAR_SRC_URL="https://github.com/rebar/rebar/archive/${REBAR_VERSION##*@}.tar.gz" \
    && REBAR_SRC_SUM="577246bafa2eb2b2c3f1d0c157408650446884555bf87901508ce71d5cc0bd07" \
    && mkdir -p /usr/src/rebar-src \
    && curl -fSL "$REBAR_SRC_URL" -o rebar-src.tar.gz \
    && echo "${REBAR_SRC_SUM}  rebar-src.tar.gz" | sha256sum -c - \
    && tar -zxf rebar-src.tar.gz -C /usr/src/rebar-src --strip-components=1 \
    && rm rebar-src.tar.gz \
    && cd /usr/src/rebar-src \
    && ./bootstrap \
    && install -v ./rebar /usr/local/bin \
    && cd /usr/src \
    && rm -rf /usr/src/rebar-src \
    && REBAR3_SRC_URL="https://github.com/erlang/rebar3/archive/${REBAR3_VERSION##*@}.tar.gz" \
    && REBAR3_SRC_SUM="49ecf89d04676d077712a10d8252bbda73998a3badf8b342481530fbc685a123" \
    && mkdir -p /usr/src/rebar3-src \
    && curl -fSL "$REBAR3_SRC_URL" -o rebar3-src.tar.gz \
    && echo "${REBAR3_SRC_SUM}  rebar3-src.tar.gz" | sha256sum -c - \
    && tar -zxf rebar3-src.tar.gz -C /usr/src/rebar3-src --strip-components=1 \
    && rm rebar3-src.tar.gz \
    && cd /usr/src/rebar3-src \
    && HOME=$PWD ./bootstrap \
    && install -v ./rebar3 /usr/local/bin \
    && cd /usr/src \
    && rm -rf /usr/src/rebar3-src \
    && ELIXIR_SRC_URL="https://github.com/elixir-lang/elixir/archive/v$ELIXIR_VERSION.tar.gz" \
    && ELIXIR_SRC_SUM="f3465d8a8e386f3e74831bf9594ee39e6dfde6aa430fe9260844cfe46aa10139" \
    && curl -fSL "$ELIXIR_SRC_URL" -o elixir.tar.gz \
    && echo "${ELIXIR_SRC_SUM}  elixir.tar.gz" | sha256sum -c - \
    && mkdir -p /usr/src/elixir-src \
    && tar -zxf elixir.tar.gz -C /usr/src/elixir-src --strip-components=1 \
    && rm -f elixir.tar.gz \
    && cd /usr/src/elixir-src \
    && make install \
    && cd / \
    && rm -rf /usr/src/elixir-src \
    && mix local.hex --force \
    && mix hex.info
