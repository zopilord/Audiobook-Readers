# Sync Text Reader for Android

The Android reader uses the same `book.json + words.tsv` folder as Desktop, Web, and Kindle. It supports local audio, Audiobookshelf streaming and progress, background playback, and fullscreen reading.

## Install the app

The release contains an APK. Open it on the phone and allow installation from that source when Android asks.

An existing build can normally be updated without uninstalling. If Android reports a signature mismatch, the new APK was signed with a different key; uninstalling will remove the app's private settings and Library entries, although the original book files remain.

## Import a book

Copy or extract the synchronized book somewhere accessible on the phone:

```text
Book Name/
├── book.json
├── words.tsv
└── Book Name.m4b   # optional
```

1. Open the app and choose **Import book**.
2. Select the book folder, not an individual JSON/TSV file.
3. If supported audio is beside `book.json`, it is attached automatically.
4. Otherwise open **Audio source → Choose M4B** or use Audiobookshelf.

Imported books are stored in the in-app Library. The app restores the last successfully opened book at startup; if it is unavailable, the Library opens instead.

Removing a Library entry does not delete the book files from the phone.

## Local audio

The same M4B used by Audiobook Workflow is the safest choice. Different editions, intros, silence trimming, or chapter gaps can move every word away from the narration.

The reader uses Android Media3 for foreground/background playback, lock-screen controls, and Bluetooth/headphone buttons.

## Audiobookshelf

Open Settings and enter the server URL and user API token.

Because the reader runs on the phone, use the server's LAN address, Tailscale IP, or MagicDNS name:

```text
http://SERVER_ADDRESS:13378
```

Then:

1. Open **Audio source**.
2. Choose/link the matching Audiobookshelf book.
3. Select **Use Audiobookshelf audio** if you want streaming.

The API token stays in Android private app storage. Do not configure it on an untrusted shared device.

Progress is saved locally and synchronized with ABS during playback, on pause, and after seeking. When local and remote positions disagree meaningfully and timestamps cannot decide which is newer, the app asks which position to use.

## Reading controls

The app includes:

- chapter selection
- tap a word to seek
- current-word color and optional full-line highlighting
- selectable reading-line position and smooth scrolling
- fullscreen mode
- text size, dark mode, warm-paper option, and color settings
- playback speed, seek bar, and sleep timer
- background audio and notification/lock-screen controls

## Updating and permissions

The app keeps access to imported folders through Android's document-provider permission. Moving, renaming, deleting, or replacing a folder can invalidate that permission; reimport the folder if the book stops opening.

Installing a compatible update preserves app data. Before uninstalling or switching signing keys, note that the Library, settings, and stored ABS token will be removed.

This project was developed with AI-assisted coding.
