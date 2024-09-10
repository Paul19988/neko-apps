# Use an Arch Linux base image
FROM archlinux:latest

# Switch to root user to install dependencies
USER root

# Initialize pacman keys and refresh the mirror list
RUN pacman-key --init && \
    pacman-key --populate archlinux && \
    pacman -Syyu --noconfirm

# Install necessary dependencies for AUR helper yay
RUN pacman -S --noconfirm \
    base-devel \
    git \
    wget \
    fuse2 && \
    rm -rf /var/cache/pacman/pkg/*

# Create a non-root user to build yay and install packages from AUR
RUN useradd -m builduser && \
    echo "builduser ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/builduser && \
    chmod 0440 /etc/sudoers.d/builduser

# Switch to non-root user for yay installation
USER builduser
WORKDIR /home/builduser

# Clone yay (AUR helper) and install it as a non-root user
RUN git clone https://aur.archlinux.org/yay.git && \
    cd yay && \
    makepkg -si --noconfirm && \
    cd && rm -rf yay

# Switch back to root user
USER root

# Use yay to install Plex Desktop from AUR
RUN yay -S --noconfirm plex-desktop

# Clean up yay and package cache
RUN yay -Yc --noconfirm && \
    rm -rf /var/cache/pacman/pkg/*

# Expose necessary ports (if applicable)
EXPOSE 32400

# Entry point to run Plex Desktop
CMD ["/usr/bin/plex-desktop"]
