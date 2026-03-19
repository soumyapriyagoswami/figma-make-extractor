<div align="center">

<picture>
  <img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=24,30,2&height=220&section=header&text=figma-extract&fontSize=72&fontAlignY=38&desc=Extract%20Images%20from%20Figma%20.make%20Files%20%E2%80%94%20Instantly&descAlignY=60&descSize=16&fontColor=ffffff&animation=fadeIn" width="100%"/>
</picture>

<br>

[![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&size=15&pause=1200&color=F9AB00&center=true&vCenter=true&width=650&lines=Zero+dependencies.+Pure+Python+stdlib.;Extracts+PNG+%7C+JPEG+%7C+GIF+%7C+WebP+at+full+resolution.;Run+locally+or+on+Google+Colab+in+seconds.;No+Figma+account.+No+API+key.+No+internet.)](https://github.com/soumyapriyagoswami/figma-extract)

<br>

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Colab](https://img.shields.io/badge/Run%20on-Google%20Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white)](https://colab.research.google.com)
[![License](https://img.shields.io/badge/License-MIT-22c55e?style=for-the-badge)](LICENSE)
[![Zero Dependencies](https://img.shields.io/badge/Dependencies-Zero-a855f7?style=for-the-badge)](#)
[![Made by Soumyapriya](https://img.shields.io/badge/Made%20with%20❤️%20by-Soumyapriya%20Goswami-ff6b9d?style=for-the-badge)](https://github.com/soumyapriyagoswami)

<br>

> ### *`.make` files are just ZIP archives in disguise — let's open them.*

<br>

---
<sub>Crafted with precision by <a href="https://github.com/soumyapriyagoswami"><b>Soumyapriya Goswami</b></a></sub>

---

</div>

<br>

## ◈ What is this?

Figma's `.make` files are ZIP archives that bundle design assets, canvas data, and embedded images. This tool **unpacks them and extracts every image at full original resolution** — no Figma account, no API key, no internet connection required.

> Works with `.make` files exported from Figma's desktop app or collaboration features.

<br>

---

## ◈ Features at a Glance

<br>

| | Feature | Details |
|---|---------|---------|
| 🖼️ | **Full Quality Extraction** | Images saved at original resolution — zero compression |
| 🔍 | **Auto Format Detection** | PNG, JPEG, GIF, WebP detected via magic bytes |
| 📐 | **Dimension Reading** | Width × Height extracted without Pillow |
| 📦 | **Zero Dependencies** | Pure Python stdlib — `zipfile`, `json`, `struct`, `shutil` |
| 🗂️ | **Metadata Parsing** | Reads `meta.json` for file name and export timestamp |
| 🖥️ | **Dual Mode** | Run as a script locally **or** as a Colab notebook |
| ⬇️ | **Colab Download** | One-click ZIP download of all images from the notebook |

<br>

---

## ◈ Project Structure

```
figma-extract/
│
├── extract_figma_images.py           ← 🐍 Standalone Python script
├── extract_figma_images_colab.ipynb  ← 📓 Google Colab notebook
├── README.md                         ← 📖 You are here
└── LICENSE
```

<br>

---

## ◈ Quick Start

<br>

### ▶ Option 1 — Run Locally

**Requirements:** Python 3.10+ · No pip installs needed

```bash
# Clone the repository
git clone https://github.com/soumyapriyagoswami/figma-extract.git
cd figma-extract

# Run the extractor
python extract_figma_images.py MyDesign.make

# Or specify a custom output folder
python extract_figma_images.py MyDesign.make ./my_images
```

**Sample output:**

```
📄 Figma file : Create Product Management Slide
🕐 Exported at: 2026-02-22T21:48:41.316Z

  [1] image_1_b089bf44.png
       Dimensions : 2560 × 2160
       Size       : 1576.8 KB
       Hash       : b089bf442014038c59d515a6a840becd59a2facb

  [2] image_2_12a8800f.png
       Dimensions : 5000 × 5000
       Size       : 1449.2 KB
       ...

✅  Done! 5 file(s) saved to: extracted_images/
```

<br>

### ▶ Option 2 — Run on Google Colab

No local setup needed. Perfect for quick extractions in the browser.

1. Open [`extract_figma_images_colab.ipynb`](extract_figma_images_colab.ipynb) in [Google Colab](https://colab.research.google.com)
2. Run cells in order:

| Step | Cell | Action |
|------|------|--------|
| 1 | **Cell 1** | 📂 Upload your `.make` file |
| 2 | **Cell 2** | ⚙️ Extract all images |
| 3 | **Cell 3** | 🖼️ Preview images inline |
| 4 | **Cell 4** | ⬇️ Download all as a `.zip` |

<br>

---

## ◈ Use as a Module

```python
from extract_figma_images import extract_figma_images

results = extract_figma_images("MyDesign.make", output_dir="./output")

# results → list of dicts, one per extracted image:
# [
#   {
#     "filename"       : "image_1_b089bf44.png",
#     "path"           : "./output/image_1_b089bf44.png",
#     "size_bytes"     : 1614660,
#     "dimensions"     : "2560x2160",
#     "original_hash"  : "b089bf442014038c59d515a6a840becd59a2facb"
#   },
#   ...
# ]
```

<br>

---

## ◈ How It Works

```
┌──────────────────────────────────────────────────────┐
│                   MyDesign.make                      │
│         ( a ZIP archive in disguise )                │
└─────────────────────┬────────────────────────────────┘
                      │  zipfile.ZipFile()
         ┌────────────▼─────────────┐
         │      Archive Contents    │
         ├──────────────────────────┤
         │  canvas.fig              │  ← Figma canvas data  (skipped)
         │  meta.json               │  ← File name, export date
         │  thumbnail.png           │  ← Preview image       ✓ extracted
         │  images/                 │
         │    ├── <sha1_hash>       │  ← Full-res image      ✓ extracted
         │    ├── <sha1_hash>       │  ← Full-res image      ✓ extracted
         │    └── <sha1_hash>       │  ← Full-res image      ✓ extracted
         └────────────┬─────────────┘
                      │
         ┌────────────▼─────────────┐
         │  Magic byte detection    │  PNG / JPG / GIF / WebP
         │  Dimension extraction    │  struct.unpack() on header
         │  Named output files      │  image_N_<hash8>.<ext>
         └──────────────────────────┘
```

<br>

---

## ◈ Supported Formats

| Format | Magic Bytes |
|--------|-------------|
| **PNG** | `\x89PNG\r\n\x1a\n` |
| **JPEG** | `\xff\xd8\xff` |
| **GIF** | `GIF87a` / `GIF89a` |
| **WebP** | `RIFF` header |

<br>

---

## ◈ Requirements

```
Python >= 3.10
No external libraries required.
```

Everything used is from the Python standard library:

```python
import zipfile, shutil, json, struct, os
from pathlib import Path
```

<br>

---

## ◈ License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

<br>

---

<div align="center">

<br>

**No Figma account. No API key. No drama.**  
*Just point it at a `.make` file and watch it work.* ✨

<br>

[![Star on GitHub](https://img.shields.io/github/stars/soumyapriyagoswami/figma-extract?style=for-the-badge&logo=github&color=F9AB00)](https://github.com/soumyapriyagoswami/figma-extract)
[![Follow Soumyapriya](https://img.shields.io/github/followers/soumyapriyagoswami?label=Follow%20Soumyapriya&style=for-the-badge&logo=github&color=ff6b9d)](https://github.com/soumyapriyagoswami)

<br>

<picture>
  <img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=24,30,2&height=100&section=footer&animation=fadeIn" width="100%"/>
</picture>

</div>
