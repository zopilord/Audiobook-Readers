# Audiobooks Readers release notes

## v2026.08.23.1

### LARK Sync for Kindle — v5.12.8

Fixed synchronized text drifting ahead during long Audiobookshelf streaming sessions.

The cause was a stream underrun while fetching the next native HTTP Range chunk: narration could pause while the fallback playback clock continued advancing. v5.12.8 now keeps a larger bounded decoded-PCM reserve and holds/re-anchors the playback clock if a real network stall empties that reserve.

Changes:

- Decoded PCM reserve increased to 2 MiB, roughly 11–12 seconds at common audiobook sample rates.
- Normal Audiobookshelf Range downloads should no longer cause the brief crackle + underline drift.
- If a longer stall empties the reserve, the playback clock stops advancing until PCM resumes, then automatically re-anchors.
- Existing cold-start anchoring and the 100 ms Sync Read update loop are preserved.
- Included v5.12.8 regression test passes all 4 checks.

The public source archive was sanitized before release: a personal Kindle Tailscale IP example and a local `/run/media/...` path were replaced with generic placeholders. No real Audiobookshelf API token was present.

All other reader packages are unchanged from the previous public release.

## v2026.08.22.1

Packages:

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
