FROM selenium/node-chrome-debug:3.4.0-francium

USER root

#===================
# Timezone settings
# Possible alternative: https://github.com/docker/docker/issues/3359#issuecomment-32150214
#===================
ENV TZ "Europe/Paris"
RUN echo "${TZ}" > /etc/timezone \
  && dpkg-reconfigure --frontend noninteractive tzdata

#=================
# Locale settings
#=================

ENV LANGUAGE fr_FR.UTF-8
ENV LANG fr_FR.UTF-8
RUN locale-gen fr_FR.UTF-8 \
  && dpkg-reconfigure --frontend noninteractive locales \
  && apt-get update -y \
  && apt-get -y --no-install-recommends install \
  language-pack-fr \
  && rm -rf /var/lib/apt/lists/* /var/cache/apt/*

#===============
# XRDP settings
#===============
RUN apt-get update -y \
  && apt-get -y install \
  xrdp \
  net-tools \
  vim \
  psmisc \
  && rm -rf /var/lib/apt/lists/* /var/cache/apt/*

ADD xrdp.ini /etc/xrdp/
RUN usermod -a -G sudo seluser

#USER seluser


RUN sed -i '/xvfb-run/s/^/sudo -H -u seluser /' /opt/bin/entry_point.sh
RUN sudo sed -i '/x11vnc /a \/etc\/init.d\/xrdp start &' /opt/bin/entry_point.sh
RUN sudo sed -i '/x11vnc /d' /opt/bin/entry_point.sh

EXPOSE 3389

