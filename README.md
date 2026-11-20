# inbox.ci.dev: a caml-list mirror using public-inbox

Serves https://inbox.ci.dev/caml-list. The archive itself is
https://github.com/mtelvers/caml-list (branch `master`); a GitHub Action on
that repo's `workflow` branch syncs it from inbox.ci.dev every 6 hours.

Services (`docker-compose.yml`):

- `public-inbox-watch` follows the list mailbox over IMAPS (see `watch` in
  `public-inbox/config`, not in git) and appends new mail to `caml-list.git`. Mail is
  pulled; nothing is delivered to this host.
- `public-inbox-httpd` serves the archive on 8080 (internal only).
- `caddy` terminates TLS for inbox.ci.dev and redirects v2.ocaml.org to
  ocaml.org.

Deployment
----------

1. `git clone https://github.com/mtelvers/caml-list-mirror && cd caml-list-mirror`
2. Copy `public-inbox/config.example` to `public-inbox/config` and set the
   real IMAP user/host in `watch`; put the IMAP credentials in
   `public-inbox/netrc`. Neither file is in git (the image
   build copies it to `/root/.netrc`).
3. Either rsync an existing `caml-list.git` (keeps the public-inbox index
   under `caml-list.git/public-inbox/`), or let `make deploy` clone it from
   GitHub and then run `public-inbox-index` inside the container.
4. Copy `watch-state/imap.sqlite3` from the old host (public-inbox-watch's
   record of which IMAP UIDs it has already imported; without it every
   message in the mailbox is fetched again). Optionally copy `caddy_data/`
   to keep the existing certificates.
5. `make deploy` (clones/fetches `caml-list.git`, then `docker compose up -d`).
