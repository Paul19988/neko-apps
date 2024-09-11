# Use the correct Neko base image from GitHub Container Registry
ARG BASE_IMAGE=ghcr.io/m1k1o/neko/base:latest
FROM $BASE_IMAGE

# Install snapd and necessary dependencies
USER root
RUN apt-get update && apt-get install -y \
    snapd \
    dbus-x11 \
    --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

# Enable snapd service
RUN systemctl enable snapd

RUN systemctl start snapd

# Create a symlink for /snap to ensure Snap is accessible
RUN ln -s /var/lib/snapd/snap /snap

# Install Plex Desktop via Snap
RUN snap install plex-desktop

# Expose necessary ports (if applicable)
EXPOSE 32400

# Switch back to non-root user
USER neko

# Start Plex Desktop
CMD ["snap", "run", "plex-desktop"]
