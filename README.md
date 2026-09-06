# Universal Virtual Print to PDF

A browser-based virtual printer that converts any printable file format - Word documents, spreadsheets, presentations, Apple iWork suites, multi-page faxes, raw scans, video contact sheets, comics, and code - into standard PDFs with 1:1 original dimensions preserved.

Runs 100% client-side inside the browser. No uploads, no servers, and zero data leakage.

Live Demo: [nekonyan.fun/anything-to-pdf](https://nekonyan.fun/anything-to-pdf)

---

## Core Philosophy

Instead of relying on heavy server-side conversion clusters or installing OS print drivers, **Anything to PDF** leverages the browser's native rendering capabilities, WebAssembly decoders, and container deconstruction techniques:

- **Zero Rescaling / Distortion:** Each file keeps its raw dimensions. An irregular phone photo or long receipt gets a canvas matching its exact pixel boundaries - no forced letterboxing or unwanted margins.
- **Container Extraction:** Modern office documents (`.docx`, `.pptx`, `.odt`) and comics (`.cbz`) are decompressed directly in memory to access raw XML and image streams.
- **High-DPI Vector Printing:** Text, markdown, and code files are laid out using a retina 2× virtual print engine with automatic line wrapping and header pagination.

---

## Supported Formats

| Category | Formats | Processing Method |
| :--- | :--- | :--- |
| **Word Processing** | `.docx`, `.rtf`, `.odt` | Visual DOM layout via `docx-preview` + `html2canvas` |
| **Spreadsheets** | `.xlsx`, `.xls`, `.ods`, `.csv`, `.tsv` | Tabular sheet parsing via `SheetJS` |
| **Presentations** | `.pptx`, `.odp` | XML slide-tree extraction to 16:9 canvas surfaces |
| **Apple iWork** | `.pages`, `.key`, `.numbers` | Internal `QuickLook/` high-res vector/raster extraction |
| **Archival Scans** | `.tif`, `.tiff` | Multi-directory IFD decoding via `UTIF.js` (all pages) |
| **Mobile & Camera** | `.heic`, `.heif`, `.dng` | In-memory transcode via `heic2any` |
| **Raster & Vector** | `.jpg`, `.jpeg`, `.jfif`, `.png`, `.webp`, `.avif`, `.bmp`, `.gif`, `.ico`, `.svg` | Direct stream embedding / Offscreen HTML5 Canvas |
| **Motion Media** | `.mp4`, `.webm`, `.mov` | Video timeline sampling into contact-sheet storyboards |
| **Digital Books** | `.cbz`, `.epub` | Alphanumeric ZIP image unpacker / XHTML chapter parser |
| **Emails & Web** | `.eml`, `.html`, `.htm` | Clean email header formatting / DOM structure render |
| **Text & Code** | `.txt`, `.md`, `.json`, `.xml`, `.yaml`, `.py`, `.js`, `.ts`, `.sql`, `.log` | 2× retina paginated text printer |

---

## Features

- **Drag-and-Drop Batch Ingestion:** Accepts loose files, folders, or `.zip` archives.
- **Reorderable Queue:** Drag handles (`☰`) to arrange file ordering before combining.
- **Actions per Document:**
  - **View in New Tab (`👁️`):** Direct in-memory blob display using native PDF viewer.
  - **Download Single (`⬇️`):** Saves the converted PDF locally.
- **Bulk Operations:**
  - **Download Combined PDF (`📑`):** Merges all processed outputs into one sequential PDF.
  - **Download as ZIP (`📦`):** Exports all PDFs inside an archive.
  - **Staggered Multi-Download:** Downloads all files individually without triggering browser download blockers.
- **Workflow Pipeline Hub:** Built-in transition link to standardize output files to A4 dimensions via **PDF-Resizer**.

---

## Repository Structure

```text
anything-to-pdf/
├── index.html          # Standalone web application
├── pdf-lib.min.js      # Core PDF transformation engine (offline)
├── jszip.min.js        # ZIP & container decompressor (offline)
└── CNAME               # Custom domain configuration (if needed)

```

---

## Setup & Offline Dependencies

### Windows (PowerShell)

```powershell
Invoke-WebRequest -Uri "[https://unpkg.com/pdf-lib/dist/pdf-lib.min.js](https://unpkg.com/pdf-lib/dist/pdf-lib.min.js)" -OutFile "pdf-lib.min.js"
Invoke-WebRequest -Uri "[https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js](https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js)" -OutFile "jszip.min.js"

```

### macOS / Linux (curl)

```bash
curl -L "[https://unpkg.com/pdf-lib/dist/pdf-lib.min.js](https://unpkg.com/pdf-lib/dist/pdf-lib.min.js)" -o pdf-lib.min.js
curl -L "[https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js](https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js)" -o jszip.min.js

```

---

## Local Development

To allow folder drag-and-drop traversal (which browsers restrict over direct `file:///` URLs):

```bash
# Python 3
python -m http.server 8000

```

Open your browser to `http://localhost:8000`.

---

## Deployment (GitHub Pages)

1. Push this repository to GitHub.
2. Navigate to **Settings** > **Pages**.
3. Under **Build and deployment**, set **Source** to `Deploy from a branch` and choose `main` / `/(root)`.
4. Configure your custom domain or `CNAME` as required.

---

## Related Tools

* **[Batch PDF Scaler & A4 Resizer](https://nekonyan.fun/PDF-Resizer):** Fit irregular document scans into standard A4 sheets or equalize all page widths with locked aspect ratios.

