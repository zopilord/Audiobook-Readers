# First public release

Packages prepared for this release:

- `audiobook-workflow-linux.zip`
- `audiobook-workflow-windows.zip`
- `audiobook-reader-linux.zip`
- `audiobook-reader-windows.zip`
- `audio-web-reader-linux.zip`
- `audio-web-reader-windows.zip`
- `audiobooksync-reader-android.apk`
- `lark-sync-kindle.zip`

Publication checks performed before packaging:

- The Web Reader archives use `HOST_TAILSCALE_IP` instead of a personal Tailscale address.
- The Kindle `audiobookshelf.conf` contains placeholder server/token values only.
- The Android APK release scan found no personal Tailscale address or embedded Audiobookshelf token.

All readers use the shared synchronized book format based on `book.json` + `words.tsv`.
