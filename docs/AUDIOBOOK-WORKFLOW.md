# Audiobook Sync Workflow — Linux + Windows

This app aligns an EPUB with its matching M4B audiobook and creates the synchronized text files used by the Desktop, Web, Android, and Kindle/LARK readers.

The canonical finished book is intentionally minimal:

```text
Book Title/
├── book.json
└── words.tsv
```

The same normal folder is used by every reader. There is no Kindle-specific `.ksr` export anymore.

The only optional finished-file extra is the local audiobook:

```text
Book Title/
├── book.json
├── words.tsv
└── Audiobook.m4b   # optional
```

## Requirements

- 64-bit Python
- FFmpeg and FFprobe
- enough free space for temporary transcription work
- an EPUB
- the matching continuous M4B audiobook

The Python environment installs PySide6, Beautiful Soup, lxml, faster-whisper, and CTranslate2.

The EPUB and M4B should represent the same edition whenever possible. Different intros, silence trimming, abridged editions, narration releases, or differently merged audio can make synchronization drift.

## Linux installation

On CachyOS / Arch:

```bash
sudo pacman -S python ffmpeg
```

Extract the workflow, open a terminal in the folder, then run:

```bash
./setup.sh
./run.sh
```

`setup.sh` creates `.venv` and installs the Python dependencies. If an NVIDIA GPU is detected, setup can also install the CUDA 12 runtime, cuBLAS, and cuDNN 9 packages used by current faster-whisper/CTranslate2 releases.

For NVIDIA GPU use on Linux, prefer `run.sh` instead of directly launching `python app.py` because the launcher exposes the required NVIDIA libraries before Python starts.

## Windows installation

1. Install 64-bit Python 3.11 or 3.12 and enable **Add Python to PATH**.
2. Install FFmpeg/FFprobe and ensure both are on PATH.
3. Extract the workflow ZIP.
4. Run `setup_windows.bat` once.
5. Start with `run_windows.bat`.

For troubleshooting, use `run_windows_debug.bat` so the terminal remains visible.

A Windows FFmpeg install can be done with:

```bat
winget install Gyan.FFmpeg
```

## NVIDIA GPU on Windows

GPU transcription is optional. CPU/int8 works without CUDA.

The **NVIDIA GPU (CUDA, float16)** profile requires current CUDA 12 cuBLAS and cuDNN 9 libraries. The Windows setup script installs the NVIDIA runtime wheels automatically when an NVIDIA GPU is detected and exposes their DLL directories from `.venv`.

At minimum the app checks for:

```text
cublas64_12.dll
cudnn64_9.dll
```

If those libraries are missing after updating, rerun `setup_windows.bat`. Otherwise select **CPU (int8)**.

## Using the workflow

1. Select the EPUB containing the authoritative book text.
2. Select the matching M4B audiobook.
3. Optionally enter the Audiobookshelf library item ID.
4. Choose the Results folder.
5. Choose the Whisper model.
6. Choose NVIDIA GPU or CPU transcription.
7. Choose Fast (beam 1) or Accurate (beam 5).
8. Choose English, Spanish, or Auto-detect.
9. Start the workflow.

`small` is a good starting Whisper model because the EPUB supplies the exact text and Whisper is primarily establishing timing.

Do not put your Audiobookshelf server URL or API token in the portable book files. Those belong in reader settings.

## Output and cache

The selected Results folder contains only finished books. Internal transcription/alignment work is kept in the OS cache so rebuilding can reuse it without cluttering the Results folder.

Linux cache:

```text
~/.cache/zopi-audiobook-sync-workflow/<book_slug>/
```

If `XDG_CACHE_HOME` is set, that location is used instead.

Windows cache:

```text
%LOCALAPPDATA%\Zopi\AudiobookSyncWorkflow\cache\<book_slug>\
```

Do not delete the cache unless you intentionally want to discard reusable transcription/alignment work.

## Finished book usage

The workflow always creates:

```text
book.json
words.tsv
```

Copy the resulting normal book folder directly to Desktop, Android, Web, or Kindle. For Kindle/LARK, place it under `/mnt/us/SyncBooks/`.

This project was developed with AI-assisted coding.
