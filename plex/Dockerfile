ARG BASE_IMAGE=m1k1o/neko:base
FROM $BASE_IMAGE

#
# install required libraries
RUN set -eux; apt-get update; \
    apt-get install -y --no-install-recommends libatk1.0-0 libatk-bridge2.0-0 libgtk-3-0 openbox; \
    #
    # clean up
    apt-get clean -y; \
    rm -rf /var/lib/apt/lists/* /var/cache/apt/* /tmp/*;

#
# install snapd and plex client
RUN set -eux; apt-get update; \
    apt-get install -y --no-install-recommends snapd; \
    snap install snapd; \
    snap install plex-desktop; \
    #
    # clean up
    apt-get clean -y; \
    rm -rf /var/lib/apt/lists/* /var/cache/apt/* /tmp/*;

#
# copy configuration files
COPY supervisord.conf /etc/neko/supervisord/plex.conf
COPY openbox.xml /etc/neko/openbox.xml