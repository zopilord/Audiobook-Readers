# Desktop Audiobook Reader

The Desktop Reader combines synchronized text, local or Audiobookshelf audio, a persistent Library, themes, fullscreen reading, and a movable always-on-top text window.

## Book format

Import a normal folder containing:

```text
Book Name/
├── book.json
└── words.tsv
```

The audiobook can be selected separately. Use the exact audio edition processed by Audiobook Workflow.

## Install on Linux

The current Linux package contains the application files directly. On CachyOS/Arch install:

```bash
sudo pacman -S --needed \
  python pyside6 python-beautifulsoup4 \
  python-gobject gtk4 gtk4-layer-shell libx11
```

Extract the ZIP, open a terminal in that folder, and run:

```bash
python3 main.py
```

Other distributions need equivalent PySide6, Beautiful Soup, GTK 4, PyGObject, layer-shell, and X11 packages. The main reader uses Qt; the Linux pop-out uses GTK and an XWayland/libX11 positioning path where available.

## Install on Windows

1. Install 64-bit Python 3.11+ from python.org and enable **Add Python to PATH**.
2. Extract the ZIP to a permanent folder.
3. Run `setup_windows.bat` once.
4. Start normally with `run_windows.bat`.

## Import and open books

1. Click **Import Book**.
2. Select the folder containing `book.json` and `words.tsv`.
3. The book is immediately added to the Library.
4. Click **Audio Source** and choose **Local M4B** or **Stream from Audiobookshelf**.
5. Press Play.

Books without audio remain valid Library entries and show **Audio not set**. Reimporting the same folder updates the entry without duplicating it or discarding its saved source.

Deleting a Library entry does not delete the synchronized folder, M4B, or Audiobookshelf item.

## Audiobookshelf

Open **Settings → Audiobookshelf** and enter:

- the server URL reachable from this computer
- the API token belonging to the correct Audiobookshelf user
- **Enable progress sync**, if desired

Press **Test connection**, then use **Audio Source → Stream from Audiobookshelf** and select the matching book.

Examples:

```text
Same computer: http://localhost:13378
LAN/Tailscale: http://SERVER_ADDRESS:13378
```

Streaming is designed around one continuous audiobook timeline. A merged M4B is recommended; multi-file items may not match the absolute word timestamps.

When a linked book reopens, the reader requests the latest Audiobookshelf position before normal playback begins. Slow streams retry the initial seek for about 30 seconds.

## Reading and playback

The reader includes:

- play/pause, seeking, chapters, search, and sleep timer
- clickable synchronized words
- fullscreen reading with a text-size control
- playback speed and saved resume position
- full-line and current-word highlighting
- dark/light themes and editable palette colors

## Pop-out text

Choose **Pop-out Text** to open the synchronized overlay.

- Drag the body to move it.
- Drag the lower-right corner to resize it.
- Position and size are saved automatically.
- Windows uses a native Qt always-on-top window.
- Linux uses GTK; KDE/Wayland may use its XWayland/libX11 positioning path.
- On Linux, a window-management rule may be required to keep the overlay always on top.

## Saved state and updating

State is portable and stored beside `main.py`:

```text
audiobook_reader_state.json
overlay_geometry.json
```

The state file includes the Library, progress, appearance settings, last book, Audiobookshelf URL, and API token.

Tested on CachyOS/KDE Plasma/Wayland and Windows 10. This project was developed with AI-assisted coding.
