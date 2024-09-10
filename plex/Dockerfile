# Use the official neko base image
FROM m1k1o/neko:debian

# Install necessary dependencies for Plex Desktop
USER root

# Install snapd and other necessary packages
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

# Make sure Plex Desktop runs as the default application in the container
USER neko

# Expose necessary ports (if applicable)
EXPOSE 32400

# Entry point to run Plex Desktop using supervisord
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]
