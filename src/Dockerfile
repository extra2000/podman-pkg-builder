FROM quay.io/centos/centos:stream

RUN dnf install -y epel-release dnf-plugins-core epel-release
RUN dnf config-manager --enable epel
RUN dnf config-manager --set-enabled powertools

RUN dnf install -y \
    sudo \
    conmon \
    containernetworking-plugins \
    containers-common \
    crun \
    device-mapper-devel \
    git \
    glib2-devel \
    glibc-devel \
    glibc-static \
    go \
    golang-github-cpuguy83-md2man \
    gpgme-devel \
    iptables \
    libassuan-devel \
    libgpg-error-devel \
    libseccomp-devel \
    libselinux-devel \
    make \
    pkgconfig \
    python3-rpkg \
    shadow-utils-subid-devel \
    ostree-devel \
    python3 \
    preproc

RUN python3 -m pip install \
    pycurl \
    munch \
    cached_property

COPY ./rpkg /opt/rpkg
WORKDIR /opt/rpkg
RUN python3 -m pip install .
RUN cp -r ./macros/macros.d /usr/lib/rpkg.macros.d

WORKDIR /opt
