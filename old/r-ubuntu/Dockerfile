## Emacs, make this -*- mode: sh; -*-

FROM ubuntu:noble

LABEL org.label-schema.license="GPL-2.0" \
      org.label-schema.vcs-url="https://github.com/rocker-org/" \
      org.label-schema.vendor="Rocker Project" \
      maintainer="Dirk Eddelbuettel <edd@debian.org>"

## Set a default user. Available via runtime flag `--user docker` 
## Add user to 'staff' group, granting them write privileges to /usr/local/lib/R/site.library
## User should also have & own a home directory (for rstudio or linked volumes to work properly). 
RUN useradd -s /bin/bash -m docker \
	&& usermod -a -G staff docker \
## Refresh apt, install minimal tools
        && apt update \
        && apt upgrade -y \
	&& apt install -y --no-install-recommends \
		ca-certificates \
		locales \
		wget \
                dirmngr \
                gpg \
                gpg-agent \
## Install key and setup marutter repo 
        && gpg --keyserver keyserver.ubuntu.com --recv-keys C8E868340AE02417DCF76B51F2B6E89C389782D0 \
        && gpg --export --armor C8E868340AE02417DCF76B51F2B6E89C389782D0 > /etc/apt/trusted.gpg.d/marutter_key.asc \
        && echo "deb [arch=amd64,arm64 signed-by=/etc/apt/trusted.gpg.d/marutter_key.asc] https://ppa.launchpadcontent.net/marutter/rrutter4.0/ubuntu noble main" > /etc/apt/sources.list.d/marutter_ppa.list \
## Configure default locale, see https://github.com/rocker-org/rocker/issues/19
        && echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
	&& locale-gen en_US.utf8 \
	&& /usr/sbin/update-locale LANG=en_US.UTF-8

## Set some variables
ENV LC_ALL=en_US.UTF-8
ENV LANG=en_US.UTF-8
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=UTC

# Now install R and littler, and create a link for littler in /usr/local/bin
# Default CRAN repo is now set by R itself, and littler knows about it too
RUN apt update \
        && apt install -y --no-install-recommends \
 		 r-base \
 		 r-base-dev \
 		 r-recommended \
## Install bspm for r2u as well as remotes and docopt used in littler script
                 r-cran-docopt \
                 r-cran-littler \
                 r-cran-remotes \
## Install python support for bspm
                 python3-dbus \
                 python3-gi \
                 python3-apt \
## But install bspm from source
        && Rscript -e 'install.packages("bspm")' \
## Support user-level installation of R packages
	&& chown root:staff "/usr/local/lib/R/site-library" \
	&& chmod g+ws "/usr/local/lib/R/site-library" \
## Install a number littler scripts
  	&& ln -s /usr/lib/R/site-library/littler/examples/build.r /usr/local/bin/build.r \
  	&& ln -s /usr/lib/R/site-library/littler/examples/check.r /usr/local/bin/check.r \
  	&& ln -s /usr/lib/R/site-library/littler/examples/install.r /usr/local/bin/install.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/install2.r /usr/local/bin/install2.r \
	&& ln -s /usr/lib/R/site-library/littler/examples/installBioc.r /usr/local/bin/installBioc.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/installDeps.r /usr/local/bin/installDeps.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/installGithub.r /usr/local/bin/installGithub.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/installRub.r /usr/local/bin/installRub.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/testInstalled.r /usr/local/bin/testInstalled.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/tt.r /usr/local/bin/tt.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/tttf.r /usr/local/bin/tttf.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/tttl.r /usr/local/bin/tttl.r \
  	&& ln -s /usr/lib/R/site-library/littler/examples/update.r /usr/local/bin/update.r \
## Configure bspm and set one helpful apt default
        && echo "options(bspm.version.check=FALSE)" >> /etc/R/Rprofile.site \
        && echo "suppressMessages(bspm::enable())" >> /etc/R/Rprofile.site \
        && echo 'APT::Install-Recommends "false";' > /etc/apt/apt.conf.d/90local-no-recommends \
	&& rm -rf /tmp/downloaded_packages/ /tmp/*.rds \
 	&& rm -rf /var/lib/apt/lists/*
