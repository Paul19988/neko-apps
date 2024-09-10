# Use the correct Neko base image from GitHub Container Registry
FROM ghcr.io/m1k1o/neko/base:latest

# Install snapd and necessary dependencies
USER root
RUN apt-get update && apt-get install -y \
    snapd \
    dbus-x11 \
    dumb-init \
    --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

# Enable systemd emulation and configure snapd
RUN systemctl enable dbus && \
    systemctl enable snapd && \
    ln -s /var/lib/snapd/snap /snap

# Install Plex Desktop via Snap
RUN snap install plex-desktop

# Ensure that snapd and Plex start automatically
USER neko

# Expose necessary ports
EXPOSE 32400

# Start snapd and Plex Desktop via dumb-init
ENTRYPOINT ["/usr/bin/dumb-init", "--"]
CMD ["snap", "run", "plex-desktop"]
