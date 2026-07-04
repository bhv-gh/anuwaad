# Anuwaad — SRT Dubbing Studio

A single-file, browser-based tool for **dubbing/voice-over from an SRT subtitle file**. Upload an `.srt`, record each subtitle line with your microphone in a hands-free "recording studio", and export one combined audio track where every take lands at its subtitle's timestamp.

*Anuwaad* (अनुवाद) means "translation" — the app is built for translating and re-voicing movie/video subtitles into another language (e.g. Konkani).

## Features

- **Upload an SRT** — auto-detects encoding (UTF-8 / UTF-16 LE/BE, with or without BOM).
- **Recording Studio (default mode)** — a focused, hands-free flow:
  - A **"get ready" countdown** before each line gives you time to compose the translation.
  - A live **Siri-style waveform** reacts to your voice while recording.
  - **Automatic silence detection** ends the take and advances to the next line — no clicking.
- **Line scrubber** — slide through all subtitles, preview any line, and **start recording from anywhere**. Slide past songs or sections that don't need dubbing.
- **Adjustable settings** — think time, silence-to-advance threshold, and mic sensitivity (saved between sessions).
- **Auto-save** — recordings and progress persist in the browser (IndexedDB); close the tab and resume later.
- **Export to MP3** — one combined, compact track (mono, 96 kbps):
  - Each take is placed at its subtitle's **start time**.
  - Takes **longer** than the subtitle's time slot are **sped up** to fit.
  - **Shorter** takes play at normal speed with **natural silence** after.
  - Skipped lines leave silence (so original song audio isn't disturbed when overlaid).
  - MP3 keeps the file ~10× smaller than uncompressed WAV for full-length films.

## Usage

No install, no build, no server:

1. Open `index.html` in **Chrome or Firefox** (both allow microphone access on local `file://` pages).
2. Enter the recording language (used for the export filename).
3. Drop in your `.srt` file.
4. Use the scrubber to pick a starting line, press **▶ Start Recording** (or `Space`), and record line by line.
5. Hit **⤓ Export** to download the combined `.mp3`.

If your browser blocks the mic on a local file, serve it instead:

```sh
python3 -m http.server 8000
# then open http://localhost:8000
```

### Keyboard shortcuts (in the studio)

| Key | Action |
|-----|--------|
| `Space` | Start / stop take · resume |
| `←` / `→` | Previous / skip line |
| `Esc` | Exit studio |

## Tech

Pure browser APIs, everything client-side:

- `getUserMedia` + `MediaRecorder` — recording
- Web Audio `AnalyserNode` — live waveform + silence detection
- `OfflineAudioContext` — offline render + timing/speed-up for export
- [lamejs](https://github.com/zhuker/lamejs) — client-side MP3 encoding (bundled as `lame.min.js`)
- IndexedDB — local persistence

Your audio never leaves your machine.
