<div align="center">

```
███████╗██╗ ██████╗ ███╗   ███╗ █████╗     ███████╗██╗  ██╗████████╗██████╗  █████╗  ██████╗████████╗
██╔════╝██║██╔════╝ ████╗ ████║██╔══██╗    ██╔════╝╚██╗██╔╝╚══██╔══╝██╔══██╗██╔══██╗██╔════╝╚══██╔══╝
█████╗  ██║██║  ███╗██╔████╔██║███████║    █████╗   ╚███╔╝    ██║   ██████╔╝███████║██║        ██║
██╔══╝  ██║██║   ██║██║╚██╔╝██║██╔══██║    ██╔══╝   ██╔██╗    ██║   ██╔══██╗██╔══██║██║        ██║
██║     ██║╚██████╔╝██║ ╚═╝ ██║██║  ██║    ███████╗██╔╝ ██╗   ██║   ██║  ██║██║  ██║╚██████╗   ██║
╚═╝     ╚═╝ ╚═════╝ ╚═╝     ╚═╝╚═╝  ╚═╝    ╚══════╝╚═╝  ╚═╝   ╚═╝   ╚═╝  ╚═╝╚═╝  ╚═╝ ╚═════╝   ╚═╝
```

# 🎨 Figma `.make` Image Extractor

**Extract high-quality embedded images from Figma `.make` files — locally or on Google Colab.**

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=flat-square&logo=python&logoColor=white)](https://python.org)
[![Colab](https://img.shields.io/badge/Run%20on-Google%20Colab-F9AB00?style=flat-square&logo=googlecolab&logoColor=white)](https://colab.research.google.com)
[![License](https://img.shields.io/badge/License-MIT-22c55e?style=flat-square)](LICENSE)
[![Zero Dependencies](https://img.shields.io/badge/Dependencies-Zero-a855f7?style=flat-square)](#)
[![Made with ❤️](https://img.shields.io/badge/Made%20with%20%E2%9D%A4%EF%B8%8F%20by-Soumyapriya%20Goswami-ff6b9d?style=flat-square)](#)

</div>

---

## ✦ What is this?

Figma's `.make` files are ZIP archives that bundle design assets, canvas data, and embedded images. This tool **unpacks them and extracts every image at full original resolution** — no Figma account, no API key, no internet connection required.

> Works with `.make` files exported from Figma's desktop app or collaboration features.

---

## ✦ Features

| Feature | Details |
|--------|---------|
| 🖼️ **Full quality extraction** | Images saved at original resolution, no compression |
| 🔍 **Auto format detection** | PNG, JPEG, GIF, WebP — detected via magic bytes |
| 📐 **Dimension reading** | Width × Height extracted without Pillow |
| 📦 **Zero dependencies** | Pure Python stdlib — `zipfile`, `json`, `struct`, `shutil` |
| 🗂️ **Metadata parsing** | Reads `meta.json` for file name and export timestamp |
| 🖥️ **Dual mode** | Run as a script locally **or** as a Colab notebook |
| ⬇️ **Colab download** | One-click ZIP download of all images from the notebook |

---

## ✦ Project Structure

```
figma-extract/
│
├── extract_figma_images.py          # 🐍 Standalone Python script
├── extract_figma_images_colab.ipynb # 📓 Google Colab notebook
├── README.md                        # 📖 You are here
└── LICENSE
```

---

## ✦ Quick Start

### ▶ Option 1 — Run Locally

**Requirements:** Python 3.10+ · No pip installs needed

```bash
# Clone the repo
git clone https://github.com/soumyapriyagoswami/figma-extract.git
cd figma-extract

# Run the extractor
python extract_figma_images.py MyDesign.make

# Or specify a custom output folder
python extract_figma_images.py MyDesign.make ./my_images
```

**Output:**
```
📄 Figma file : Create Product Management Slide
🕐 Exported at: 2026-02-22T21:48:41.316Z

  [1] image_1_b089bf44.png
       Dimensions : 2560×2160
       Size       : 1576.8 KB
       Hash       : b089bf442014038c59d515a6a840becd59a2facb

  [2] image_2_12a8800f.png
       Dimensions : 5000×5000
       Size       : 1449.2 KB
       ...

✅ Done! 5 file(s) saved to: extracted_images/
```

---

### ▶ Option 2 — Run on Google Colab

No local setup needed. Perfect for quick extractions in the browser.

1. Open [`extract_figma_images_colab.ipynb`](extract_figma_images_colab.ipynb) in [Google Colab](https://colab.research.google.com)
2. Run the cells in order:

| Cell | Action |
|------|--------|
| **Cell 1** | 📂 Upload your `.make` file |
| **Cell 2** | ⚙️ Extract all images |
| **Cell 3** | 🖼️ Preview images inline |
| **Cell 4** | ⬇️ Download all as a `.zip` |

---

## ✦ Use as a Module

```python
from extract_figma_images import extract_figma_images

results = extract_figma_images("MyDesign.make", output_dir="./output")

# results is a list of dicts:
# [
#   {
#     "filename": "image_1_b089bf44.png",
#     "path": "./output/image_1_b089bf44.png",
#     "size_bytes": 1614660,
#     "dimensions": "2560x2160",
#     "original_hash": "b089bf442014038c59d515a6a840becd59a2facb"
#   },
#   ...
# ]
```

---

## ✦ How It Works

```
┌─────────────────────────────────────────────────────┐
│                  MyDesign.make                       │
│  (it's just a ZIP file in disguise)                  │
└──────────────────────┬──────────────────────────────┘
                       │  zipfile.ZipFile()
          ┌────────────▼────────────┐
          │     Archive Contents    │
          ├─────────────────────────┤
          │  canvas.fig             │ ← Figma canvas data (skipped)
          │  meta.json              │ ← File name, export date
          │  thumbnail.png          │ ← Preview image ✓ extracted
          │  images/                │
          │    ├── <sha1_hash>      │ ← Full res image ✓ extracted
          │    ├── <sha1_hash>      │ ← Full res image ✓ extracted
          │    └── <sha1_hash>      │ ← Full res image ✓ extracted
          └─────────────────────────┘
                       │
          ┌────────────▼────────────┐
          │   Magic byte detection  │ PNG / JPG / GIF / WebP
          │   Dimension extraction  │ struct.unpack() on header
          │   Named output files    │ image_N_<hash8>.<ext>
          └─────────────────────────┘
```

---

## ✦ Supported Formats

| Format | Detection |
|--------|-----------|
| PNG | `\x89PNG\r\n\x1a\n` magic bytes |
| JPEG | `\xff\xd8\xff` magic bytes |
| GIF | `GIF87a` / `GIF89a` |
| WebP | `RIFF` header |

---

## ✦ Requirements

```
Python >= 3.10
No external libraries required.
```

All modules used are from the Python standard library:

```python
import zipfile, shutil, json, struct, os
from pathlib import Path
```

---

## ✦ License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

crafted with precision by **Soumyapriya Goswami**

*If this saved you time, consider giving it a ⭐*

</div>
