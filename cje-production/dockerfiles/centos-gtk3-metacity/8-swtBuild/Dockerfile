ARG FROM_TAG="8"
FROM centos:${FROM_TAG}

### user name recognition at runtime w/ an arbitrary uid - for OpenShift deployments
COPY scripts/uid_entrypoint /usr/local/bin/uid_entrypoint
RUN chmod u+x /usr/local/bin/uid_entrypoint && \
    chgrp 0 /usr/local/bin/uid_entrypoint && \
    chmod g=u /usr/local/bin/uid_entrypoint /etc/passwd
### end

ENTRYPOINT [ "uid_entrypoint" ]
RUN yum -y update && yum -y install \
      gtk3 \
      tigervnc-server \
      tigervnc \
      metacity \
      xorg-x11-server-utils \
      mesa-libGL \
      xorg-x11-fonts-misc \
      xorg-x11-fonts-75dpi \
      xorg-x11-fonts-Type1 \
      xorg-x11-fonts-100dpi \
      liberation-fonts \
      gnu-free-fonts-common \
      dejavu-fonts-common \
      unzip \
      zip \
      bzip2 \
      lsof \
      wget \
      curl \
      webkitgtk4 \
      webkitgtk4-devel \
      gtk3-devel \
      gtk2 \
      gtk2-devel \
      java-1.8.0-openjdk-devel \
      webkit2gtk3-devel \
      make \
      gcc \
      mesa-libGLU-devel \
      mesa-libGL-devel \
      libXt-devel \
      libXtst-devel \
      libXScrnSaver \
      GConf2 \
      cmake \
      freetype \
      glibc-locale-source \
      glibc-langpack-en \
      nss \
      nspr \
      nss-util \
    && yum clean all

# -i: specify the locale definition file
# -f: specify the character set
RUN localedef -i en_US -f UTF-8 en_US.UTF-8
ENV LANG=en_US.UTF-8

ENV HOME=/home/vnc
ENV DISPLAY :0

RUN mkdir -p ${HOME}/.vnc && chmod -R 775 ${HOME} \
  && echo "123456" | vncpasswd -f > ${HOME}/.vnc/passwd \
  && chmod 600 ${HOME}/.vnc/passwd

# Create a custom vnc xstartup file
COPY scripts/xstartup_metacity.sh ${HOME}/.vnc/xstartup.sh
RUN chmod 755 ${HOME}/.vnc/xstartup.sh

USER 10001
