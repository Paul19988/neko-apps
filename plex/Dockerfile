# Use the correct Neko base image from GitHub Container Registry
FROM ghcr.io/m1k1o/neko/base:latest

# Switch to root user to install dependencies
USER root

# Install necessary dependencies for AUR package installation
RUN apt-get update && apt-get install -y \
    git \
    build-essential \
    --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

# Install yay (AUR helper)
RUN git clone https://aur.archlinux.org/yay.git /opt/yay && \
    cd /opt/yay && \
    makepkg -si --noconfirm && \
    cd && rm -rf /opt/yay

# Install Plex Desktop from AUR using yay
RUN yay -S --noconfirm plex-desktop

# Clean up yay cache and unnecessary packages
RUN yay -Yc --noconfirm

# Switch back to neko user
USER neko

# Expose necessary ports (if applicable)
EXPOSE 32400

# Entry point to run Plex Desktop using supervisord
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]
