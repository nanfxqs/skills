---
name: markitdown
description: Use this skill whenever you need to read or extract content from PDF, Excel, Word, PowerPoint, or other binary document files by converting them to Markdown format. Triggers include: wanting to read the text content of a .pdf, .xlsx, .xls, .docx, .doc, .pptx, .ppt, .html, .htm, .csv, .json, .xml, .zip, .epub, or other supported file types; needing to extract tables, text, or structured data from documents; wanting to quickly inspect document contents without specialized libraries. Use the `markitdown` CLI tool via `conda activate markitdown` environment. Do NOT use for plain text or code files that can be read directly.
---

# MarkItDown - Document to Markdown Converter

## Overview

MarkItDown is a Microsoft tool that converts various document formats to Markdown, making it easy to read and extract content from binary files like PDFs, Excel spreadsheets, Word documents, PowerPoint presentations, and more.

## When to Use This Skill

Use this skill when you need to **read or extract content** from any of the following file types:

- **PDF** (`.pdf`) — Extract text, tables, and structure
- **Excel** (`.xlsx`, `.xls`) — Convert spreadsheets to Markdown tables
- **Word** (`.docx`, `.doc`) — Extract document content
- **PowerPoint** (`.pptx`, `.ppt`) — Extract slide content
- **HTML** (`.html`, `.htm`) — Convert web pages to Markdown
- **CSV** (`.csv`) — Tabular data conversion
- **JSON** (`.json`) — Structured data to Markdown
- **XML** (`.xml`) — Markup data conversion
- **EPUB** (`.epub`) — E-book content extraction
- **ZIP** (`.zip`) — Inspect archive contents
- **Images** (`.jpg`, `.png`, `.gif`, `.tiff`, `.bmp`) — EXIF and metadata extraction (with optional OCR)

## Environment Setup & Teardown

**IMPORTANT**: The `markitdown` tool is installed in a dedicated conda environment. You must save your current environment, switch to markitdown, and **restore the original environment** after use.

### Step 1: Save current environment (before use)

Record the name of the currently active environment so you can restore it later:

```bash
PREV_ENV=$(conda info --envs | grep '*' | awk '{print $1}')
```

If `$CONDA_DEFAULT_ENV` is available, you can also use:

```bash
echo $CONDA_DEFAULT_ENV
```

### Step 2: Activate markitdown

```bash
conda activate markitdown
```

### Step 3: Restore previous environment (after use)

**Do NOT just run `conda deactivate`** — it only drops one level in the conda env stack and may not return to your original environment (e.g., if you were in `MECC`, `conda deactivate` goes to `base`, not `MECC`).

Instead, explicitly activate the previously saved environment:

```bash
conda activate "$PREV_ENV"
```

**Why**: `conda deactivate` only pops the conda env stack by one level. If your previous environment was not `base`, `conda deactivate` will leave you in `base` instead of your actual working environment. Always restore by name to guarantee you return to the correct environment.

## Usage

### Basic Conversion (stdout)

Convert a file and output Markdown to stdout:

```bash
markitdown path-to-file.pdf
```

### Save to File

Use `-o` to specify an output file:

```bash
markitdown path-to-file.pdf -o document.md
```

### Pipe Input

You can also pipe content into markitdown:

```bash
cat path-to-file.pdf | markitdown
```

### Common Examples

```bash
# Convert a PDF to Markdown and save
markitdown report.pdf -o report.md

# Convert an Excel spreadsheet
markitdown data.xlsx -o data.md

# Convert a Word document
markitdown contract.docx -o contract.md

# Convert a PowerPoint presentation
markitdown slides.pptx -o slides.md

# Convert and read HTML
markitdown page.html -o page.md

# Quick preview of a file (to stdout)
markitdown document.pdf
```

## Workflow

When you need to read a document file:

1. **Save current environment**: `PREV_ENV=$(conda info --envs | grep '*' | awk '{print $1}')`
2. **Activate markitdown**: `conda activate markitdown`
3. **Convert to Markdown**: `markitdown <input-file> -o <output>.md`
4. **Read the output**: Use the Read tool to read the generated `.md` file
5. **Process content**: Work with the Markdown content as needed
6. **Restore previous environment**: `conda activate "$PREV_ENV"`

### Quick One-Liner for Reading

If you just need to quickly read a file's content without saving:

```bash
PREV_ENV=$(conda info --envs | grep '*' | awk '{print $1}') && conda activate markitdown && markitdown path-to-file.pdf && conda activate "$PREV_ENV"
```

The Markdown output will appear in stdout and can be captured directly.

## Important Notes

- **Always `conda activate markitdown` before running `markitdown`** — the tool is installed in this specific conda environment
- **Always restore the previous environment after use** — save the current env name first (`PREV_ENV=$(conda info --envs | grep '*' | awk '{print $1}')`), then restore with `conda activate "$PREV_ENV"`. Do NOT just run `conda deactivate` — it drops to `base`, not your original working environment (e.g., `MECC`)
- For complex PDFs with heavy graphics or unusual layouts, the text extraction may not be perfect — consider using the `pdf` skill for advanced PDF operations
- For Excel files with complex formulas or macros, the `xlsx` skill may provide more precise control
- For Word documents requiring precise editing, use the `docx` skill instead
- For PowerPoint editing or creation, use the `pptx` skill instead
- The strength of `markitdown` is **quick content extraction** across many formats using a single, simple command