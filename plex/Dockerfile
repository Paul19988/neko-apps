# Use an Arch Linux base image
FROM archlinux:latest

# Switch to root user to install dependencies
USER root

# Initialize pacman keys and refresh the mirror list
RUN pacman-key --init && \
    pacman-key --populate archlinux && \
    pacman -Syyu --noconfirm

# Install necessary dependencies for AUR helper yay and dbus
RUN pacman -S --noconfirm \
    base-devel \
    git \
    wget \
    dbus \
    alsa-lib \
    libgl \
    hicolor-icon-theme \
    fontconfig \
    freetype2 \
    harfbuzz \
    lcms2 \
    libdrm \
    libjpeg-turbo \
    libwebp \
    libx11 \
    libxcb \
    libxcomposite \
    libxdamage \
    libxext \
    libxfixes \
    libxkbcommon \
    libxkbfile \
    libxrandr \
    libxslt \
    libxtst \
    mesa \
    minizip \
    nspr \
    nss \
    opus \
    xcb-util-renderutil \
    libxss \
    xcb-util-image \
    libxkbcommon-x11 \
    libxinerama \
    xcb-util-keysyms \
    xcb-util-wm \
    snappy \
    libva \
    libpulse \
    libxrender \
    wayland \
    qt6-base && \
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

# Set yay options to automatically choose default packages for libvpx and jack
RUN yay --save --answerdiff None --answerclean None

# Switch back to root user
USER root

# Enable and start dbus service to bypass systemd checks
RUN dbus-uuidgen > /etc/machine-id && \
    dbus-daemon --system

# Download PKGBUILD files manually for plex-desktop and ffmpeg6.1
RUN git clone https://aur.archlinux.org/plex-desktop.git /opt/plex-desktop && \
    git clone https://aur.archlinux.org/ffmpeg6.1.git /opt/ffmpeg6.1

# Change ownership of the directories to builduser
RUN chown -R builduser:builduser /opt/plex-desktop /opt/ffmpeg6.1

# Switch back to non-root user to build and install ffmpeg6.1 first
USER builduser
RUN cd /opt/ffmpeg6.1 && \
    makepkg -si --noconfirm

# Build and install plex-desktop manually
RUN cd /opt/plex-desktop && \
    makepkg -si --noconfirm

# Switch back to root user
USER root

# Clean up yay and package cache
RUN yay -Yc --noconfirm && \
    rm -rf /var/cache/pacman/pkg/*

# Expose necessary ports (if applicable)
EXPOSE 32400

# Entry point to run Plex Desktop
CMD ["/usr/bin/plex-desktop"]
