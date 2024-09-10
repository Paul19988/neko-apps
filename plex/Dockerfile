# Use the official Neko base image from GitHub Container Registry
FROM ghcr.io/m1k1o/neko/base:latest

# Switch to root user to install dependencies
USER root

# Install snapd and other necessary packages for Plex Desktop
RUN apt-get update && apt-get install -y \
    snapd \
    dbus-x11 \
    --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

# Enable and start snapd service
RUN systemctl enable dbus && \
    systemctl enable snapd

# Install Plex Desktop client using snap
RUN snap install plex-desktop

# Ensure that snapd and Plex start automatically
USER neko

# Expose necessary ports (if applicable)
EXPOSE 32400

# Entry point to run Plex Desktop using supervisord
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]
