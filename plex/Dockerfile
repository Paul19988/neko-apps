# Use the correct Neko base image from GitHub Container Registry
ARG BASE_IMAGE=ghcr.io/m1k1o/neko:base
FROM $BASE_IMAGE

LABEL m1k1o.neko_rooms.api_version=2

# Install Flatpak and necessary dependencies
USER root
RUN apt-get update && apt-get install -y \
    flatpak \
    xdg-desktop-portal \
    xdg-desktop-portal-gtk \
    --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

RUN apt-get install -y dbus

RUN mkdir -p /var/run/dbus

RUN dbus-daemon --config-file=/usr/share/dbus-1/system.conf --print-address

# Add Flathub repository
RUN flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# Install Plex Desktop from Flathub
RUN flatpak install -y flathub tv.plex.PlexHTPC

# Expose necessary ports (if applicable)
EXPOSE 32400

# Set environment variables for Flatpak
ENV XDG_RUNTIME_DIR=/run/user/1000

# copy configuation files
COPY supervisord.conf /etc/neko/supervisord/plex.conf

# Run Plex Desktop from Flatpak
RUN flatpak run tv.plex.PlexHTPC
