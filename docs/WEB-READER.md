# LARK Web Reader

One computer runs LARK Web Reader as the host. Phones and other computers open it in a browser and use the same transcript library, covers, listening position, Audiobookshelf links, and deletions.

## Requirements

- Node.js 22.13 or newer and npm
- Current Firefox, Chrome, Edge, Brave, or mobile browser
- Optional Tailscale for private remote access
- Optional Audiobookshelf on host port `13378`

## Install on Linux

On CachyOS/Arch:

```bash
sudo pacman -S --needed nodejs npm unzip
```

Extract the ZIP to a permanent folder, enter it, and run:

```bash
bash run-local.sh
```

The first start installs the Node dependencies. Keep the terminal open and visit:

```text
http://localhost:4173/
```

## Install on Windows

1. Install Node.js 22 LTS or newer and keep **Add to PATH** enabled.
2. Extract the ZIP to a permanent folder.
3. Double-click `run-local.cmd`.
4. Allow Node.js on **Private networks** if Windows Firewall asks.
5. Open `http://localhost:4173/`.

Stop the host with `Ctrl+C` in its terminal.

## Open it on another device

Keep the host awake and LARK running. Connect both devices to the same Tailscale network, find the host address with:

```bash
tailscale ip -4
```

Then open:

```text
http://HOST_TAILSCALE_IP:4173/
```

## Import and use a book

Select **Import book folder** and choose:

```text
Book Name/
├── book.json
├── words.tsv
├── Book Name.m4b   # optional
└── cover.jpg       # optional
```

Choose the book from the Library, select local audio or Audiobookshelf, and press Play. The reader includes chapter navigation, clickable words, ±30 seconds, speed, volume, text size, fullscreen, sleep timer, themes, and media controls.

## Audiobookshelf

LARK proxies Audiobookshelf through `/abs`, avoiding direct cross-origin browser requests.

In each browser open **Settings → Audiobookshelf** and use:

```text
Host:         http://localhost:4173/abs
Other device: http://HOST_TAILSCALE_IP:4173/abs
```

Paste that browser's API token, test the connection, then link the LARK book to the matching ABS item. The book link is shared; the token is intentionally not shared.

The recommended Audiobookshelf container mapping is:

```yaml
ports:
  - "13378:80"
```

`ALLOW_CORS=1` is not required when the LARK `/abs` address is used.

When moving between devices, pause on the first device before opening or focusing LARK on the second. Focusing or refreshing requests newer ABS progress.

This project was developed with AI-assisted coding.
