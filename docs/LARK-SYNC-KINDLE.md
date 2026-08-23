# LARK Sync for Kindle

Current Kindle release: **LARK Sync v5.12.8**.

Tested on a jailbroken Kindle Paperwhite 4 running firmware 5.18.1.1.1.

LARK Sync plays local M4B files or streams from Audiobookshelf. **SYNC READ** is optional and uses the same `book.json + words.tsv` folder as every other reader.

## v5.12.8 — streaming sync recovery

Long Audiobookshelf streaming sessions could previously drift out of sync after a slow HTTP Range download. Audio could briefly crackle or pause while the fallback playback clock kept advancing, leaving the synchronized text roughly a line ahead until LARK restarted.

v5.12.8 fixes this in two ways:

- The decoded PCM reserve is now bounded at 2 MiB, roughly 11–12 seconds at common audiobook sample rates, so normal Range downloads do not reach the speaker.
- If a longer network stall really empties the reserve, the playback clock holds instead of advancing through silence, then automatically re-anchors when PCM output resumes.

The existing first-audio anchor and 100 ms Sync Read update loop are preserved.

## Requirements

- Compatible jailbroken/native-enabled Kindle
- KUAL, Scriptlets, or another way to launch native software
- Working Bluetooth audio
- LARK Sync release files
- Optional Tailscale and Audiobookshelf

Audio playback does not require synchronized text. The text folder is needed only for **SYNC READ**.

## Install Tailscale on Your Kindle

https://tailscale.com/blog/tailscale-jailbroken-kindle

## Install a prebuilt release

After extracting the release, copy:

```text
LARK-Sync/  -> Kindle/LARK-Sync/
```

It must become:

```text
/mnt/us/LARK-Sync/
```

Copy the supplied `.sh` Scriptlets from `documents/` into:

```text
/mnt/us/documents/
```

Launch **LARK Sync Reader**, or use SSH/KTerm:

```bash
cd /mnt/us/LARK-Sync
./start_lark_sync.sh
```

## Copy books

Recommended Kindle layout:

```text
/mnt/us/
├── LARK-Sync/
├── SyncBooks/
│   └── Book Name/
│       ├── book.json
│       └── words.tsv
└── Audiobooks/
    └── Book Name.m4b
```

The M4B is optional when streaming from Audiobookshelf. The SyncBooks folder is optional when only listening to audio.

With Tailscale/SSH:

```bash
scp -r "/path/to/Book Name" root@$KINDLE:/mnt/us/SyncBooks/
scp "/path/to/Book Name.m4b" root@$KINDLE:/mnt/us/Audiobooks/
```

With USB, copy the same folders to `Kindle/SyncBooks/` and `Kindle/Audiobooks/`, then safely eject.

Open **SOURCE** to switch between local M4B and Audiobookshelf. If an ABS book has no local file, **LOCAL M4B** opens a storage browser beginning under `/mnt/us/Audiobooks/` when that folder exists.

The ABS Library supports title/author search and cached grayscale covers. Covers are stored under:

```text
/mnt/us/LARK-Sync/cache/covers/
```

## Audiobookshelf over Tailscale

Create an Audiobookshelf API key under **Settings → Users → API Keys**. Use the Tailscale IP or MagicDNS name of the machine running Audiobookshelf.

Create:

```text
/mnt/us/LARK-Sync/audiobookshelf.conf
```

with:

```ini
enabled=true
server=http://AUDIOBOOKSHELF_TAILSCALE_IP:13378
token=YOUR_API_KEY
proxy=http://127.0.0.1:1055
```

## Sync Read behavior

Open the matching audio, link/select the synchronized folder when asked, choose the text size, and press **SYNC READ**.

LARK uses an E-Ink-friendly current-line indicator rather than repainting every word. The 100 ms synchronization timer exists only while Sync Read is open and stops immediately when the reader closes.

## Update safely

Before updating, back up:

```text
/mnt/us/LARK-Sync/audiobookshelf.conf
```

Then close LARK, replace the `LARK-Sync/` folder and Scriptlets with the new release, restore the private configuration, and start LARK again.

The main state database is:

```text
/mnt/us/.lark_player.db
```
