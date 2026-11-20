FROM debian:trixie

# public-inbox-watch (IMAP -> caml-list.git) and public-inbox-httpd.
# Mail is pulled over IMAPS; no MTA is needed.
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get -y install --no-install-recommends \
      ca-certificates \
      public-inbox \
      libdbd-sqlite3-perl \
      libsearch-xapian-perl \
      xapian-tools \
      libmail-imapclient-perl \
      libio-socket-ssl-perl \
      liblinux-inotify2-perl \
      libplack-perl \
      libplack-middleware-reverseproxy-perl \
      libtimedate-perl \
      libemail-address-xs-perl \
      libinline-c-perl && \
    rm -rf /var/lib/apt/lists/*

COPY config /root/.public-inbox/config
COPY ocaml.css /root/.public-inbox/ocaml.css
COPY netrc /root/.netrc
RUN chmod 600 /root/.netrc
