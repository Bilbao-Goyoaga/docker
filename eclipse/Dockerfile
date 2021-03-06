FROM bilbaogoyoaga/x11_jdk8

MAINTAINER bilbao.goyoaga@gmail.com

## -------------------------------------------------------------------------------
## ---- USER_NAME is defined in parent image: openkbs/jdk-mvn-py3-x11 already ----
## -------------------------------------------------------------------------------
ARG USER_NAME=${USER_NAME:-developer}
ENV USER_NAME=${USER_NAME}
ARG HOME=${ECLIPSE_HOME:-/home/${USER_NAME}}
ENV HOME=${ECLIPSE_HOME}
ENV ECLIPSE_WORKSPACE=${HOME}/eclipse-workspace


## ----------------------------------------------------------------------------
## ---- To change to different Eclipse version: e.g., oxygen, change here! ----
## ----------------------------------------------------------------------------

## -- 1.) Eclipse version: oxygen, photon, etc.: -- ##
ARG ECLIPSE_VERSION=${ECLIPSE_VERSION:-photon}
ENV ECLIPSE_VERSION=${ECLIPSE_VERSION}

## -- 2.) Eclipse Type: -- ##
ARG ECLIPSE_TYPE=${ECLIPSE_TYPE:-jee}
#ARG ECLIPSE_TYPE=${ECLIPSE_TYPE:-modeling}

## -- 3.) Eclipse Release: -- ##
ARG ECLIPSE_RELEASE=${ECLIPSE_RELEASE:-R}
#ARG ECLIPSE_RELEASE=${ECLIPSE_RELEASE:-2}

## -- 4.) Eclipse Download Mirror site: -- ##
#ARG ECLIPSE_OS_BUILD=${ECLIPSE_OS_BUILD:-win32-x86_64}
ARG ECLIPSE_OS_BUILD=${ECLIPSE_OS_BUILD:-linux-gtk-x86_64}

## -- 5.) Eclipse Download Mirror site: -- ##
#http://mirror.math.princeton.edu/pub/eclipse/technology/epp/downloads/release/photon/R/eclipse-jee-photon-R-linux-gtk-x86_64.tar.gz
#http://mirror.math.princeton.edu/pub/eclipse/technology/epp/downloads/release/photon/R/eclipse-modeling-photon-R-linux-gtk-x86_64.tar.gz
ARG ECLIPSE_MIRROR_SITE_URL=${ECLIPSE_MIRROR_SITE_URL:-http://mirror.math.princeton.edu}

## ----------------------------------------------------------------------------------- ##
## ----------------------------------------------------------------------------------- ##
## ----------- Don't change below unless Eclipse download system change -------------- ##
## ----------------------------------------------------------------------------------- ##
## ----------------------------------------------------------------------------------- ##
## -- Eclipse TAR/GZ filename: -- ##
#ARG ECLIPSE_TAR=${ECLIPSE_TAR:-eclipse-jee-photon-R-linux-gtk-x86_64.tar.gz}
ARG ECLIPSE_TAR=${ECLIPSE_TAR:-eclipse-${ECLIPSE_TYPE}-${ECLIPSE_VERSION}-${ECLIPSE_RELEASE}-${ECLIPSE_OS_BUILD}.tar.gz}

## -- Eclipse Download route: -- ##
ARG ECLIPSE_DOWNLOAD_ROUTE=${ECLIPSE_DOWNLOAD_ROUTE:-pub/eclipse/technology/epp/downloads/release/${ECLIPSE_VERSION}/${ECLIPSE_RELEASE}}

## -- Eclipse Download full URL: -- ##
## e.g.: http://mirror.math.princeton.edu/pub/eclipse/technology/epp/downloads/release/photon/R/
## e.g.: http://mirror.math.princeton.edu/pub/eclipse/technology/epp/downloads/release/photon/R/
ARG ECLIPSE_DOWNLOAD_URL=${ECLIPSE_DOWNLOAD_URL:-${ECLIPSE_MIRROR_SITE_URL}/${ECLIPSE_DOWNLOAD_ROUTE}}

## http://ftp.osuosl.org/pub/eclipse/technology/epp/downloads/release/photon/R/eclipse-jee-photon-R-linux-gtk-x86_64.tar.gz
## http://mirror.math.princeton.edu/pub/eclipse/technology/epp/downloads/release/photon/R/eclipse-jee-photon-R-linux-gtk-x86_64.tar.gz
## http://mirror.math.princeton.edu/pub/eclipse/technology/epp/downloads/release/photon/R/eclipse-modeling-photon-R-linux-gtk-x86_64.tar.gz
WORKDIR /opt
RUN sudo wget -q -c ${ECLIPSE_DOWNLOAD_URL}/${ECLIPSE_TAR} && \
    sudo tar xvf ${ECLIPSE_TAR} && \
    sudo rm ${ECLIPSE_TAR}

#################################
#### Install Eclipse Plugins ####
#################################
# ... add Eclipse plugin - installation here (see example in https://github.com/DrSnowbird/papyrus-sysml-docker)
# RUN cd /opt/eclipse && \
#     ./eclipse -application org.eclipse.equinox.p2.director -repository http://download.eclipse.org/releases/oxygen/ -installIU org.eclipse.acceleo.feature.group -nosplash && \
#     ./eclipse -application org.eclipse.equinox.p2.director -repository http://download.eclipse.org/releases/oxygen/ -installIU org.eclipse.m2m.atl.feature.group -nosplash && \
#     ./eclipse -application org.eclipse.equinox.p2.director -repository http://download.eclipse.org/modeling/mdt/papyrus/updates/releases/oxygen,http://download.eclipse.org/modeling/mdt/papyrus/components/bpmn/oxygen,http://download.eclipse.org/modeling/mdt/papyrus/components/sysml14/oxygen,http://download.eclipse.org/releases/oxygen -installIU org.eclipse.papyrus.sdk.feature.feature.group,org.eclipse.papyrus.toolsmiths.feature.feature.group,org.eclipse.papyrus.sysml14.feature.feature.group,org.eclipse.papyrus.bpmn.feature.feature.group -nosplash

RUN mkdir -p ${HOME}/.eclipse ${ECLIPSE_WORKSPACE} &&\
    groupadd -g 999 vboxsf && \
    useradd -d /home/${USER_NAME} -m ${USER_NAME} && \
    adduser ${USER_NAME} vboxsf && \
    sudo chown -R ${USER_NAME}:${USER_NAME} ${ECLIPSE_WORKSPACE} ${HOME}/.eclipse

USER ${USER_NAME}
WORKDIR ${ECLIPSE_WORKSPACE}
CMD ["/opt/eclipse/eclipse"]
