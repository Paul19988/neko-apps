# Use the correct Neko base image from GitHub Container Registry
FROM ghcr.io/m1k1o/neko/base:latest

# Install snapd and necessary dependencies
USER root
RUN apt-get update && apt-get install -y \
    snapd \
    dbus-x11 \
    fuse \
    squashfs-tools \
    --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

# Create necessary directories and symlinks for snapd
RUN mkdir -p /var/lib/snapd/seed /var/lib/snapd/snaps /var/snap && \
    ln -s /var/lib/snapd/snap /snap

# Manually start snapd and install Plex Desktop via Snap
RUN systemctl is-active --quiet snapd || (snap install core && snap install plex-desktop)

# Expose necessary ports (if applicable)
EXPOSE 32400

# Start Snapd manually and run Plex Desktop in the foreground
CMD ["/bin/sh", "-c", "/usr/lib/snapd/snapd & sleep 10 && snap run plex-desktop"]
