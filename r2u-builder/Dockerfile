## Emacs, make this -*- mode: sh; -*-

FROM rocker/r-ubuntu:24.04

LABEL org.label-schema.license="GPL-2.0" \
      org.label-schema.vcs-url="https://github.com/eddelbuettel/r2u" \
      maintainer="Dirk Eddelbuettel <edd@debian.org>"

## This was not needed before but we need it now
ENV DEBIAN_FRONTEND=noninteractive

RUN rm -f /etc/apt/sources.list.d/edd-ubuntu-misc-jammy.list \
    && apt update -qq \
    && apt upgrade --yes \
    && apt install --yes --no-install-recommends dh-r gpg-agent xvfb 

COPY debBuild.sh /usr/local/bin
COPY r2u.sources /etc/apt/sources.list.d
COPY 99cranapt /etc/apt/preferences.d

RUN apt update -qq && apt upgrade --yes

CMD ["bash"]
