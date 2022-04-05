# ----------------------------------
# Pterodactyl Panel Dockerfile
# Environment: Source Engine
# ----------------------------------
FROM        debian:buster-slim

LABEL       author="Pterodactyl Software - edited by DRANIX" maintainer="dranix@hypesrv.com"

ENV         DEBIAN_FRONTEND noninteractive
ENV         TERM screen

# from postgressql - set up en_US.UTF8 locale so srcds doesn't whine
# https://github.com/docker-library/postgres/blob/69bc540ecfffecce72d49fa7e4a46680350037f9/9.6/Dockerfile#L21-L24
RUN         tput setaf 2; echo "Setting en_US.UTF8 locale..."; tput sgr0;   \
            apt-get update                                                  \
            && apt-get install -y locales --no-install-recommends           \
            && rm -rf /var/lib/apt/lists/*                                  \
            && localedef -i en_US -c -f UTF-8                               \
            -A /usr/share/locale/locale.alias en_US.UTF-8

ENV         LANG en_US.utf8

# install deps, create our container user, and be done with it
RUN         tput setaf 2; echo "Installing dependencies..."; tput sgr0;     \
            dpkg --add-architecture i386                                    \
            && apt-get update                                               \
            && apt-get install -y --no-install-recommends                   \
            # needed for ip route stuff in entrypoint.sh
            net-tools iproute2                                              \
            # TF2 Wiki says these are required
            lib32z1 libncurses5:i386 libbz2-1.0:i386 lib32gcc1              \
            lib32stdc++6 libtinfo5:i386 libcurl3-gnutls:i386 lib32stdc++6   \
            # needed for sdl threading
            libsdl2-2.0-0:i386                                              \
            # needed for some sourcemod extensions
            curl libcurl4:i386                                              \
            # needed for steamcmd
            ca-certificates;                                                \
# clean our package cache to keep img size down
            tput setaf 2; echo "Cleaning packages..."; tput sgr0;           \
            apt-get autoremove -y                                           \
            && apt-get clean                                                \
            && rm -rf /var/lib/apt/lists/*;                                 \
# create our container user
            tput setaf 2; echo "Creating container user..."; tput sgr0;     \
            useradd -U -m -d /home/container -s /bin/bash container;        \
            tput setaf 2; echo "Done!"; tput sgr0;

USER        container:container
ENV         HOME /home/container
WORKDIR     /home/container

COPY        ./entrypoint.sh /entrypoint.sh
CMD         ["/bin/bash", "/entrypoint.sh"]
