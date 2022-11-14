FROM ubuntu:focal

# set environment variables for tzdata
ARG TZ=America/New_York
ENV TZ=${TZ}

# include manual pages and documentation
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update &&\
  yes | unminimize

# install GCC-related packages
RUN apt-get -y install\
 binutils-doc\
 cpp-doc\
 gcc-doc\
 g++\
 g++-multilib\
 gdb\
 gdb-doc\
 glibc-doc\
 libblas-dev\
 liblapack-dev\
 liblapack-doc\
 libstdc++-10-doc\
 make\
 make-doc

# install clang-related packages
RUN apt-get -y install\
 clang\
 clang-10-doc\
 lldb\
 clang-format

# install cmake packages
RUN apt-get -y install\
  cmake\
  libssl-dev\
  libboost-all-dev\
  doctest-dev\
  doxygen\
  libcrypto++-dev\
  libcrypto++-doc\
  libcrypto++-utils\
  libsqlite3-dev

# install programs used for system exploration
RUN apt-get -y install\
 blktrace\
 linux-tools-generic\
 strace\
 tcpdump

# install interactive programs (emacs, vim, nano, man, sudo, etc.)
RUN apt-get -y install\
 bc\
 curl\
 dc\
 git\
 git-doc\
 man\
 micro\
 nano\
 psmisc\
 sudo\
 wget

# install rust
RUN RUSTUP_HOME=/opt/rust CARGO_HOME=/opt/rust \
  bash -c "curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sudo -E sh -s -- -y"

# set up libraries
RUN apt-get -y install\
 libreadline-dev\
 locales\
 wamerican\
 libssl-dev

# install programs used for networking
RUN apt-get -y install\
 dnsutils\
 inetutils-ping\
 iproute2\
 net-tools\
 netcat\
 telnet\
 time\
 traceroute

# set up default locale
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8

# remove unneeded .deb files
RUN rm -r /var/lib/apt/lists/*

# set up passwordless sudo for user cs1515-user
RUN useradd -m -s /bin/bash cs1515-user && \
  echo "cs1515-user ALL=(ALL:ALL) NOPASSWD: ALL" > /etc/sudoers.d/cs1515-init

# create binary reporting version of dockerfile
RUN (echo '#\!/bin/sh'; echo 'echo 1') > /usr/bin/cs1515-docker-version; chmod ugo+rx,u+w,go-w /usr/bin/cs1515-docker-version

# git build arguments
ARG USER=cs1515\ User
ARG EMAIL=nobody@example.com

# configure your environment
USER cs1515-user
RUN rm -f ~/.bash_logout

WORKDIR /home/cs1515-user
CMD ["/bin/bash", "-l"]
