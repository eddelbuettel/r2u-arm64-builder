## Emacs, make this -*- mode: sh; -*-

FROM eddelbuettel/r2u_build:noble

LABEL org.label-schema.license="GPL-2.0" \
      org.label-schema.vcs-url="https://github.com/eddelbuettel/r2u" \
      maintainer="Dirk Eddelbuettel <edd@debian.org>"

## This was not needed before but we need it now
ENV DEBIAN_FRONTEND=noninteractive

## Packages needed for r2u,
## and for docker which r2u checks for even though we do not use it here
## and for libapt-pkg-dev as we need to force a reinstallation of RcppAPT
RUN rm -f /etc/apt/sources.list.d/edd-ubuntu-misc-jammy.list \
    && apt update -qq \
    && apt upgrade --yes \
    && apt install --yes --no-install-recommends r-cran-remotes r-cran-data.table r-cran-crayon docker.io libapt-pkg-dev

## Update r2u.r script until littler is updated
COPY r2u_0.0.9.tar.gz /tmp
COPY r2u.r /usr/local/bin/

RUN R CMD INSTALL /tmp/r2u_0.0.9.tar.gz \
    && rm -v /tmp/r2u_0.0.9.tar.gz \
    && r -e 'r2u:::.createDefaultConfiguration()' \
    && install.r RcppAPT

CMD ["bash"]
