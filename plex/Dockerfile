# Use an Arch Linux base image
FROM archlinux:latest

# Switch to root user to install dependencies
USER root

# Install necessary dependencies including AUR helper yay
RUN pacman -Syu --noconfirm && \
    pacman -S --noconfirm \
    base-devel \
    git \
    wget \
    fuse2 \
    libfuse && \
    rm -rf /var/cache/pacman/pkg/*

# Clone yay (AUR helper) and install it
RUN git clone https://aur.archlinux.org/yay.git /opt/yay && \
    cd /opt/yay && \
    makepkg -si --noconfirm && \
    cd && rm -rf /opt/yay

# Use yay to install Plex Desktop from AUR
RUN yay -S --noconfirm plex-desktop

# Clean up yay and package cache
RUN yay -Yc --noconfirm && \
    rm -rf /var/cache/pacman/pkg/*

# Switch back to non-root user if needed (adjust depending on Neko setup)
#USER neko

# Expose necessary ports (if applicable)
EXPOSE 32400

# Entry point to run Plex Desktop using supervisord or any script
CMD ["/usr/bin/plex-desktop"]
