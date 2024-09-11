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

# Start snapd and install Plex Desktop
RUN /usr/lib/snapd/snapd & \
    sleep 10 && \
    snap install plex-desktop

# Expose necessary ports (if applicable)
EXPOSE 32400

# Use dumb-init to manage the snapd service and start Plex Desktop
USER neko
ENTRYPOINT ["/usr/bin/dumb-init", "--"]

# Start snapd and run Plex Desktop
CMD ["/bin/sh", "-c", "/usr/lib/snapd/snapd & sleep 5 && snap run plex-desktop"]
