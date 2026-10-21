# Metadata Cleaner

> **Inspect, remove, and verify metadata and AI-generation evidence from your files — with privacy-first processing.**

[![Platform](https://img.shields.io/badge/Platform-Android%20%7C%20Linux%20%7C%20Docker-blue.svg)](https://github.com)
[![Privacy](https://img.shields.io/badge/Privacy-100%25%20On--Device-success.svg)](https://github.com)
[![Verification](https://img.shields.io/badge/Verification-Independent%20Audit-emerald.svg)](https://github.com)
[![License](https://img.shields.io/badge/License-Apache%202.0-lightgrey.svg)](LICENSE)

**Metadata Cleaner (aimc)** is a professional, privacy-focused security utility engineered to discover, explain, sanitize, and independently verify hidden metadata, geotags, and generative AI parameter footprints in images and multimedia files.

---

## 🧭 Core Workflow: Scan → Explain → Clean → Verify

Unlike conventional metadata strippers that silently overwrite files or make unverified claims, Metadata Cleaner operates on a transparent four-phase privacy pipeline:

```text
       ┌───────────────────────────┐
       │        UPLOAD FILE        │
       └─────────────┬─────────────┘
                     ▼
       ┌───────────────────────────┐
       │     1. PRIVACY SCAN       │
       │  (EXIF, XMP, GPS, AI Gen) │
       └─────────────┬─────────────┘
                     ▼
       ┌───────────────────────────┐
       │    2. EXPLAIN FINDINGS    │
       │   (Exposure Meter & Risk) │
       └─────────────┬─────────────┘
                     ▼
       ┌───────────────────────────┐
       │   3. REMOVE METADATA      │
       │  (Pixel-Pristine Remux)   │
       └─────────────┬─────────────┘
                     ▼
       ┌───────────────────────────┐
       │   4. VERIFY CLEAN FILE    │
       │  (Independent 2nd Scan)   │
       └─────────────┬─────────────┘
                     ▼
       ┌───────────────────────────┐
       │    DOWNLOAD CLEAN FILE    │
       │    [ VERIFIED CLEAN ✓ ]   │
       └───────────────────────────┘
```

---

## 🛡️ Key Features

### 1. Privacy Scanner
- **Privacy Exposure Meter**: Categorizes exposure as `LOW`, `MEDIUM`, or `HIGH` (`████████░░ HIGH`). Explains that the indicator represents detected metadata exposure rather than an absolute security guarantee.
- **Deep Standards Extraction**:
  - **EXIF**: Camera make, camera model, serial number, lens configuration, aperture, shutter speed, ISO, focal length, timestamps.
  - **GPS**: High-precision latitude, longitude, altitude, and satellite sync timestamps.
  - **XMP / IPTC**: Extensible Metadata Platform packets, creator credits, copyright notices, editing history.
  - **Embedded Other**: Embedded preview thumbnails, ICC color profiles, and XML chunks.

### 2. Generative AI Evidence & Parameter Detection
- **Strict Evidence Principle**: We do **not** claim *"This image was definitely created by AI"*. Instead, the app precisely reports *"AI generation metadata detected"* with confidence classified as *"Metadata evidence only"*.
- **Supported AI Frameworks**:
  - **Stable Diffusion (WebUI / Automatic1111 / Forge)**: Positive prompts, negative prompts, seed, steps, sampler scheduler, CFG scale, base checkpoint, and LoRA weights.
  - **ComfyUI**: Visual node graphs, prompt pipelines, and workflow canvas state.
  - **Midjourney**: Prompt text, aspect ratios (`--ar`), version flags (`--v`), stylize multipliers (`--s`), and chaos seeds.
  - **NovelAI**: Generation configurations and prompt structures.
  - **OpenAI / DALL-E / Firefly**: DigitalSourceType (`trainedAlgorithmicMedia`) attribution tags.
  - **C2PA / Content Credentials**: Detects cryptographic provenance manifests and CAI assertion blocks.

### 3. Independent Verification System
- After sanitization, an independent second scan inspects the output file across 7 verification vectors:
  1. `✓ EXIF scan`
  2. `✓ XMP scan`
  3. `✓ IPTC scan`
  4. `✓ GPS scan`
  5. `✓ AI metadata scan`
  6. `✓ C2PA scan`
  7. `✓ Embedded metadata scan`
- Distinguishes **`VERIFIED CLEAN`** from **`VERIFICATION INCOMPLETE`**. If any metadata remains, the exact residual fields are reported.

### 4. Side-by-Side Before / After Comparison
- Visual status indicators:
  - `✓ Removed`: Successfully stripped and confirmed absent in verification.
  - `⚠ Preserved`: Kept per user settings.
  - `— Not present`: Not found in the original file.
  - `? Unable to verify`: Could not independently verify.

### 5. Batch Sanitization
- Select and process up to 50 files simultaneously.
- Queue breakdown: files with metadata, files with GPS, files with AI metadata, and files with C2PA manifests.
- Post-processing summary: `X processed, Y verified clean, Z partially cleaned, W failed`.

### 6. Exportable Privacy Inspection Reports
- Generates structured audit reports with sensitive field counts, detection breakdown, and verification proofs.
- One-tap export to formatted plain text or structured JSON.

### 7. Direct WebAssembly (Wasm) Browser Engine
- **Zero-Install Client Processing**: Full offline metadata inspection and sanitization inside any modern web browser (Chrome, Safari, Firefox, Edge).
- **Isolated Linear Memory (`WebAssembly.Memory`)**: Files never leave client RAM. Strict `Content-Security-Policy: default-src 'none'` air-gap guarantee ensures zero network transmission.
- **In-App Interactive Sandbox**: An embedded air-gapped Android WebView runtime allows live testing, performance audits (< 2ms JIT init, ~1.2ms scan), and verification without external dependencies.
- **Standalone HTML5 Export**: One-tap export and sharing of `metadata-cleaner-wasm.html` — a self-contained, single-file zero-install web app for desktop PCs, Macs, and iOS devices.

### 8. Lossless Video Sanitization (MP4 / MOV)
- Strips embedded container user-data (`udta`) metadata atoms and ISO 6709 geolocation tags (`@xyz`) without re-encoding video or audio streams.
- Zero generational quality loss — streams are extracted and remuxed losslessly in milliseconds.

### 9. Expanded RAW Profile Sanitization (CR3, ARW, NEF, CR2, DNG)
- **Deep Hardware Footprint Purge**: Professional camera RAW files store hardware serial numbers, lens IDs, shutter count, and precise GPS coordinates inside proprietary MakerNote structures.
- **Canon RAW 3 (CR3)**: ISOBMFF container parsing purges Canon metadata UUID blocks (CMT1, CMT2, CMT3, CMT4 GPS) while preserving sensor raw Bayer payload.
- **Sony ARW & Nikon NEF**: Traverses IFD0 and Exif SubIFD directories to strip GPS sub-IFDs, camera serials in MakerNotes, photographer/copyright tags, and XMP packets.
- **Lossless Sensor Preservation**: Sensor geometry, CFA repeat patterns, black/white balance levels, and raw Bayer streams remain completely intact.

### 10. Hardware-Bound Cryptographic Sanitization Proofs
- **Secure Hardware Attestation**: Generates an asymmetric ECDSA P-256 keypair isolated inside the Android KeyStore (backed by TEE - Trusted Execution Environment or StrongBox Secure Element).
- **Cryptographic Attestation Certificate**: Every sanitization links original SHA-256 and cleaned SHA-256 digests, timestamps, and verification audit results into a mathematically signed proof.
- **Live Signature Verification**: In-app audit tool recalculates SHA-256 digests and verifies the ECDSA signature against the device's public key, confirming untampered provenance.
- **Export Formats**: One-tap export to standardized JSON-LD attestation certificates and plain text audit reports.

---

## 📂 Supported Formats Matrix

| Category | File Extensions | Support Level | Capabilities |
| :--- | :--- | :--- | :--- |
| **Images** | `JPG`, `JPEG`, `PNG`, `WebP`, `HEIC`, `AVIF`, `TIFF` | **Supported** | Full scan, pixel reconstruction, metadata purge & secondary verification |
| **Video** | `MP4`, `MOV` | **Supported** | Native lossless container remuxing, GPS geotag (@xyz) & UDTA metadata purge |
| **RAW** | `CR3`, `ARW`, `NEF`, `DNG` | **Supported** | Deep profile sanitization: purges camera/lens serials in MakerNotes, GPS & XMP |
| **RAW** | `CR2` | **Scan Only** | Format detected; MakerNote purge not yet covered by a dedicated verification test |
| **Other Video** | `M4V`, `MKV`, `WebM` | **Scan Only** | Container metadata, GPS track & codec profile extraction |
| **Documents** | `PDF`, `DOCX`, `XLSX`, `PPTX` | **Scan Only** | Creator, author, revision count, and modification dates |
| **Audio** | `MP3`, `M4A`, `WAV` | **Scan Only** | ID3 tags, artist, album, and encoder software |

---

## 🔒 Privacy Model & Security Architecture

1. **100% On-Device Processing**:
   - `● Local Processing`: All file reading, chunk parsing, pixel decompression, and re-encoding occur purely in volatile device memory.
   - Zero network requests, zero remote analytics, and zero cloud backups.
2. **Untrusted Input Hardening**:
   - **Magic Byte Validation**: Sniffs true binary file headers (`FF D8 FF` for JPEG, `89 50 4E 47` for PNG, `RIFF...WEBP` for WebP, `%PDF-` for PDF) rather than trusting user-provided file extensions or MIME types.
   - **Path Traversal Protection**: Replaces directory traversal sequences (`../`, `..\\`), null bytes (`\0`), and illegal filesystem characters.
   - **Canonical Path Bounds**: Verifies that written output files remain strictly inside app-internal cache bounds.
   - **Resource Limits**: 150MB maximum file size guards against memory starvation and decompression bombs.
   - **Log Redaction**: Diagnostic logs never output GPS coordinates, prompts, creator names, or file payloads.

---

## 💻 CLI (aimc) — planned

A standalone `aimc` CLI is **planned but not yet shipped**. When implemented it will expose:

```bash
# Scan a photo for hidden metadata and AI parameters
aimc scan photo.jpg

# Clean an image
aimc clean photo.jpg --output ./sanitized

# Independently verify a cleaned image
aimc verify ./sanitized/clean_photo.jpg

# Batch sanitize an entire folder
aimc clean ./photos --output ./clean-photos
```

Example intended output (subject to change when implemented):
```text
AI Metadata Cleaner (aimc)
Scanning: photo.jpg [JPEG Image]
----------------------------------------
GPS:            FOUND [⚠ High Privacy Exposure]
Camera/Device:  FOUND [⚠]
XMP:            FOUND [⚠]
AI metadata:    FOUND [⚠ Evidence detected]
C2PA:           NOT FOUND [✓]
----------------------------------------
Cleaning...
✓ EXIF removed
✓ XMP removed
✓ GPS wiped
✓ AI parameters stripped

Verifying output...
✓ EXIF scan: PASSED
✓ GPS scan: PASSED
✓ XMP scan: PASSED
✓ AI metadata scan: PASSED
✓ C2PA scan: PASSED

RESULT: ✓ VERIFIED CLEAN
```

---

## 🧰 Packaging & Deployment

### Android app
The primary artifact is the on-device Android application (Compose UI, Room history, KeyStore-backed attestation). Build the debug APK:

```bash
gradle :app:assembleDebug
```

The APK lands at `app/build/outputs/apk/debug/app-debug.apk`.

### Headless CLI / server engine — planned
The README and docs reference a standalone `aimc` CLI (`aimc scan`, `aimc clean`, `aimc verify`) and a Docker/REST microservice. Those artifacts are **not yet shipped** in this repository — the `AimcCli` object lives inside the Android app, and the Dockerfile currently prints a readiness notice rather than running a server. When implemented, the CLI/server will consume the same `MetadataExtractor` / `MetadataCleaner` / `MetadataVerifier` pipeline used by the app.

### WebAssembly (Wasm) export
The app can export `metadata-cleaner-wasm.html` — a single-file, zero-install browser tool that runs the same scan/clean/verify pipeline inside any modern browser. Files stay in browser memory; there is no network call from the Wasm tool.

---

## 🛠️ Testing & Verification

Comprehensive local JVM unit tests are implemented using JUnit:

```bash
# Run unit tests
gradle :app:testDebugUnitTest

# Assemble debug APK
gradle :app:assembleDebug
```

### Test Coverage (what's covered today)
- File signature & magic bytes validation (JPEG, PNG, WebP, PDF, RAW, MP4, MOV)
- Malicious filename sanitization and path traversal prevention
- Stable Diffusion (Automatic1111 / WebUI / Forge) parameter extraction ✔
- Midjourney flags and prompt parsing ✔
- C2PA manifest detection ✔
- Independent verification pass/fail audit and residual-XMP / residual-AI detection ✔
- Privacy inspection report text and JSON generation ✔
- Expanded camera RAW profile format detection (CR3, ARW, NEF, DNG) and MakerNote purge verification ✔
- Hardware-bound cryptographic ECDSA P-256 signing, attestation validation, tampering rejection & JSON export ✔

### Not yet covered by dedicated tests (detected in code, but no parser/verification test)
- ComfyUI node-graph parsing
- NovelAI JSON parameter parsing
- DALL-E / OpenAI / Adobe Firefly `trainedAlgorithmicMedia` detection
- CR2 MakerNote sanitization
- MP4/MOV lossless remux end-to-end verification

---

## 🗺️ Roadmap

- [x] Independent secondary verification engine (`MetadataVerifier`)
- [x] Before / After comparison breakdown
- [x] Magic byte & file signature security validation
- [x] Exportable Privacy Inspection Reports (Text & JSON)
- [x] Batch processing with verification statistics directly on Clean tab
- [x] Direct WebAssembly (Wasm) browser engine for zero-install client processing
- [x] Lossless video stream remuxing for MP4/MOV container stripping
- [x] Expanded RAW profile sanitization (CR3, ARW, NEF, CR2, DNG)
- [x] Hardware-bound cryptographic signing of sanitization proofs
- [ ] Standalone `aimc` CLI binary (`scan` / `clean` / `verify`)
- [ ] Docker + REST microservice wrapping the same engine pipeline
- [ ] Video-specific verification tests for MP4/MOV lossless remux
- [ ] Dedicated parser/verification tests for CR2, ComfyUI, NovelAI, DALL-E/Firefly detection paths

---

## 📄 License & Attribution

Licensed under the [Apache License, Version 2.0](LICENSE).

Copyright © 2026 Mohd Ismail Mat Asin (mohdismailmatasin@gmail.com).
