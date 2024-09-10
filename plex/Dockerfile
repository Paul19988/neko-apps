# Use the correct Neko base image from GitHub Container Registry
FROM ghcr.io/m1k1o/neko/base:latest

# Switch to root user to install dependencies
USER root

# Install dependencies for running AppImages
RUN apt-get update && apt-get install -y \
    wget \
    fuse \
    libfuse2 \
    --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

# Download Plex Desktop AppImage
RUN wget https://plex.tv/downloads/latest/5 -O /usr/local/bin/plex-desktop.AppImage

# Make the AppImage executable
RUN chmod +x /usr/local/bin/plex-desktop.AppImage

# Create a script to run Plex Desktop
RUN echo '#!/bin/bash\n/usr/local/bin/plex-desktop.AppImage' > /usr/local/bin/start-plex.sh && chmod +x /usr/local/bin/start-plex.sh

# Switch back to neko user
USER neko

# Expose necessary ports (if applicable)
EXPOSE 32400

# Entry point to run Plex Desktop using supervisord
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]
