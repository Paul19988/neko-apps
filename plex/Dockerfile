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

# Create a symlink for /snap to ensure Snap is accessible
RUN ln -s /var/lib/snapd/snap /snap

# Install Plex Desktop via Snap
RUN snap install plex-desktop

# Expose necessary ports (if applicable)
EXPOSE 32400

# Start Snapd with dumb-init to simulate systemd environment and run Plex Desktop
USER neko
ENTRYPOINT ["/usr/bin/dumb-init", "--"]
CMD ["snap", "run", "plex-desktop"]
