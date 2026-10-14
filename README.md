# Metamorphosis

**Local image, video, and PDF toolkit for Windows.**
Resize and convert images without visible quality loss, compress and convert video, and compress, merge, or condense
PDFs. Everything runs on your own computer. No file is ever uploaded to the internet.

[Bahasa Indonesia](README.md)

| | |
|---|---|
| **Platform** | Windows 10 / 11 (x64). Phones and tablets connect through the browser over local WiFi. |
| **Version** | 2.4.0 |
| **Interface** | Bahasa Indonesia, light and dark themes, desktop and mobile layouts |
| **Privacy** | 100% offline processing. The server listens on `127.0.0.1` only, unless phone access is turned on. |

---

## Contents

- [Highlights](#highlights)
- [Installation](#installation)
- [Getting started](#getting-started)
- [Images](#images)
- [Video](#video)
- [PDF](#pdf)
- [Using it from a phone](#using-it-from-a-phone)

---

## Highlights

- **Images:** downscale and convert between 10 output formats, including HEIC, AVIF, WEBP, and ICO, with Lanczos
  resampling and sharpening that keeps results crisp.
- **Target file size:** give a size limit in KB or MB and the app finds the highest quality that fits.
- **Video:** compress to MP4 (H.264 / HEVC) or WEBM (VP9), convert to GIF, or extract MP3 audio, with optional trimming.
- **PDF:** compress embedded images only, merge files by drag and drop, or condense a document onto fewer pages
  while text stays as sharp, selectable vector text.
- **Phone access:** scan a QR code and use the same app from a phone browser, protected by a random access key.
- **Simple to ship:** a single `.exe` with no console window, runs from the system tray, with no Python or FFmpeg
  installation required.

---

## Installation

Two distribution options are available. Users only need one of them.

| File | Use it when |
|---|---|
| `installer\Metamorphosis-Setup.exe` | You want a normal installation with a Start Menu entry and an optional desktop shortcut. |
| `Metamorphosis.exe` | You want a portable app. Copy it anywhere, such as the desktop or a USB drive, and double click it. |

The installer:

- Does not require administrator rights. It installs per user into `%LOCALAPPDATA%\Programs\Metamorphosis`.
- Automatically removes the previous product name (AAChangeIMG Pro) if it is present.
- Can be removed through **Settings → Apps → Installed apps → Metamorphosis → Uninstall**.

> The first launch after installation can take longer than usual while Windows Defender scans the new executable.

---

## Getting started

1. Open **Metamorphosis** from the Start Menu, or double click `Metamorphosis.exe`.
2. Your default browser opens the app automatically. No console window appears. A tray icon appears near the clock.
3. Choose a tab: **Gambar** (Images), **Video**, or **PDF**. You can also drop files anywhere on the window.
   Files are routed to the right tab automatically. Images can also be pasted from the clipboard.
4. Adjust the options in the side panel and press **Proses** (Process).
5. Preview a result with the eye icon, download it individually, or download everything as a ZIP.

To exit, click **Keluar** (Quit) in the top right corner, or right click the tray icon and choose **Keluar**.
Starting the app again while it is already running simply opens a new browser tab.

---

## Images

### Supported formats

| Input | Output |
|---|---|
| JPG / JPEG / JFIF, PNG, WEBP, AVIF, HEIC / HEIF, GIF, BMP, TIFF, ICO, PSD, TGA, PCX, PPM, DDS, ICNS, JPEG 2000, QOI, and more | JPG, PNG, WEBP, AVIF, HEIC, ICO, BMP, TIFF, GIF, PDF |

### Resize modes

| Mode (UI label) | Behaviour |
|---|---|
| **Tetap** (Original) | No resizing. Convert or recompress only. |
| **Persen** (Percent) | 1 to 100% of the original dimensions. |
| **Piksel** (Pixels) | Maximum width and height with the aspect ratio preserved, or an exact size when the ratio lock is off. Presets are available. |
| **Maks file** (Max file size) | A target size in KB or MB. The app searches for the highest quality that fits. If that is still too large, it lowers the resolution only as much as needed (binary search), so the image never turns blurry or blocky. |

Images are **never upscaled**. If the requested size is larger than the original, the original size is kept.

### How quality is preserved

- **Full Lanczos resampling** without the reducing gap shortcut, using premultiplied alpha so transparent edges
  do not darken.
- **Light unsharp mask** after downscaling to restore perceived sharpness. You can turn it off or increase it.
- **4:4:4 chroma** for JPG, AVIF, and HEIC at quality 80 and above, which keeps colored edges and text sharp.
- **EXIF orientation** is applied, **ICC color profiles** are preserved, and CMYK is converted to sRGB through ICC.
- **Animation** in GIF, WEBP, APNG, and AVIF is preserved when the output format supports it.
- **Transparency** is preserved for PNG, WEBP, AVIF, HEIC, TIFF, ICO, and GIF. For JPG, BMP, and PDF, the
  background is filled with a color of your choice.
- In file size mode, the minimum quality is calibrated per encoder (JPG / WEBP q70, AVIF / HEIC q50) so results
  stay visually equivalent.

Measured on a 3840×2400 photo downscaled by 50%: PSNR of 44.9 dB for JPG, 44.2 dB for AVIF, 41.8 dB for WEBP, and
above 50 dB for HEIC. Values above 40 dB are generally indistinguishable from the source to the human eye.

Additional options include keeping camera metadata (EXIF), lossless WEBP, PNG color reduction, and selectable ICO
icon sizes.

---

## Video

FFmpeg is bundled, so nothing else needs to be installed.

| Output | Details |
|---|---|
| **MP4** (H.264) | Plays on virtually every device. |
| **HEVC** (H.265) | Smaller files at the same visual quality. |
| **WEBM** (VP9) | An open format for the web. |
| **GIF** | Width from 320 to 800 px and 10 to 25 frames per second. A palette is generated per video (palettegen + paletteuse) for smooth gradients. |
| **MP3** | Extracts the audio track. |

- **Quality:** **Tinggi** (High), **Seimbang** (Balanced), and **Kecil** (Small) use CRF for constant quality.
  **Maks file** (Max file size) uses two pass encoding to hit the target size precisely. If the target is very small,
  the resolution is lowered automatically to avoid blocky artifacts.
- **Resolution:** downscale only, using Lanczos. Rotation metadata from phone videos is respected.
- **GIF size targets:** the frame rate is reduced to 15 fps first, then the width.
- **MP3 bitrates:** 320, 192, or 128 kbps, or a target file size. The bitrate never exceeds what the source audio
  needs, with a cap of 160 kbps for mono. 5.1 audio is downmixed to stereo, and title and other tags are kept.
- **Trim:** set **Mulai** (Start) and **Durasi** (Duration), for example `0:15`. This works for every output format.
- Upload and encoding progress are shown live, and jobs can be cancelled at any time.

---

## PDF

| Mode (UI label) | What it does |
|---|---|
| **Kompres** (Compress) | Downsamples embedded images only: High (200 DPI), Balanced (150 DPI), Small (100 DPI), or a target file size. Text, vector graphics, and fonts are left untouched. |
| **Gabung** (Merge) | Combines two or more PDFs. Reorder them by dragging rows, or with Alt+↑ / Alt+↓ on the keyboard. |
| **Ringkas** (Condense) | Fits the content onto fewer pages, for example a 2 page document onto 1 page. |

**Compress**

- Diagrams and screenshots stay lossless. Black and white scans, CMYK images, and JPEG 2000 images are left as they are.
- The output is never larger than the original file.

**Merge**

- Optional bookmark per source file, and optional compression of the merged result.
- Identical images shared across files are stored only once.
- The output file name can be set, and it is sanitized automatically.

**Condense**

- Empty space above and below the content on each page is removed. Content blocks are then stacked in order and
  scaled down only as much as needed.
- Horizontal position is preserved, so a signature on the right stays on the right.
- Text stays as vector text, so it remains sharp, searchable, and selectable.
- Options: target page count, paper size (Original, A4, F4, Letter, Legal), and margin (Thin 5 mm, Normal 10 mm,
  Wide 20 mm).
- Blank pages are skipped, and rotated pages are handled correctly.
- If the requested page count would shrink text below 35% of its original size, the app uses the smallest page count
  that remains readable and shows a note explaining why.

Password protected PDFs are rejected with a clear message. The PDF engine is built on pikepdf (qpdf) and pypdfium2,
both under permissive licenses.

---

## Using it from a phone

A Windows `.exe` cannot be installed on Android or iOS. Instead, a phone uses the copy of Metamorphosis running on
the computer, through the phone's browser. Nothing needs to be installed on the phone.

1. On the computer, click the **phone icon** in the top right corner and turn on
   **Izinkan HP di WiFi yang sama** (Allow phones on the same WiFi).
2. Scan the QR code with the phone camera. The phone and the computer must be on the same WiFi network.
3. If Windows asks for network permission the first time, choose **Allow** for private networks.

The mobile layout is designed for touch: 40 to 48 px touch targets, 16 px input text so iOS does not zoom, card style
file rows on narrow screens, and sticky header and action buttons. The app can be added to the home screen through
the browser menu.

### Security model

- **Off by default.** Phone access turns off again every time the app starts.
- **Random access key.** The QR code contains a random key. Devices without it get an access denied page, even on
  the same network. The key is stored in an HttpOnly cookie and removed from the address bar immediately.
- **30 day sessions.** A phone that has scanned the code stays connected for 30 days.
  **Putuskan semua HP** (Disconnect all phones) generates a new key and signs out every phone.
- **Limited remote rights.** Phones cannot quit the app or change the phone access settings.
- **Separate listener.** The main server stays bound to `127.0.0.1`. Phone access runs as a second listener on the
  computer's LAN address only while it is turned on.

The computer must stay on, with Metamorphosis running, while a phone is using it.

---

The app then opens at <http://127.0.0.1:5000>.

Optional environment variables:

| Variable | Default | Purpose |
|---|---|---|
| `PORT` | `5000` | Preferred port. The next free port is used if it is taken. |
| `NO_BROWSER` | unset | Set to `1` to skip opening the browser. |

For phone access, use the built in feature described above rather than `HOST=0.0.0.0`, which exposes the app without
an access key.
