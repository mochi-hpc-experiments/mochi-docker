FROM ubuntu

ARG USERNAME=mochi

# install as many dependency packages as we can from Ubuntu
RUN apt-get update \
 && apt-get -y upgrade \
 && apt-get -y install \
 gcc automake cmake libtool pkgconf \
 git python3 bison fuse sudo vim curl \
 libfuse-dev libssl-dev \
 libfmt-dev libjson-c-dev mpich \
 nlohmann-json3-dev libspdlog-dev libtclap-dev

# add a user, no password, with sudo privileges
RUN useradd -m -s /bin/bash $USERNAME \
 && echo "$USERNAME ALL=(ALL:ALL) NOPASSWD: ALL" >> /etc/sudoers.d/$USERNAME \
 # delete passwd
 && passwd -d $USERNAME

# Set up Spack. Note that we select a specific version
# (v0.19.2 in this case) to avoid unexpected incompatibilities
USER $USERNAME
RUN cd \
 && git clone -b v0.19.2 -c feature.manyFiles=true --depth 1 https://github.com/spack/spack.git \
 && . spack/share/spack/setup-env.sh \
 && spack bootstrap now \
 && printf '%s\n' \
    '. $HOME/spack/share/spack/setup-env.sh' \
    >> .bashrc

# add Mochi spack repository
COPY configs/packages.yaml /home/$USERNAME/.spack/packages.yaml
RUN cd \
 && git clone --depth 1 https://github.com/mochi-hpc/mochi-spack-packages.git \
 && . spack/share/spack/setup-env.sh \
 && spack repo add mochi-spack-packages

# NOTE: we intentionally do not actually install any Mochi-specific
# spack packages.  We save that step for the hands-on exercises

WORKDIR /home/$USERNAME

# sleep indefinitely when the container starts so that we can connect to it
# with interactive sessions
CMD sleep infinity
