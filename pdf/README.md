# PDF Plugin

Interactive PDF workflows — view, annotate, fill forms, and sign
documents in a live viewer with real-time visual feedback.

## What It Does

- **Open PDFs** from local files or academic sources (arXiv, bioRxiv, etc.)
- **Annotate collaboratively** — Claude proposes highlights, notes, and
  stamps section by section; you review each batch in the viewer
- **Fill forms** — guided field-by-field completion with live preview
- **Sign documents** — place signature/initials images on the page
- **Stamp approvals** — APPROVED, DRAFT, CONFIDENTIAL, or any custom label
- **Download** — export the annotated PDF from the viewer toolbar

## Commands

| Command | What it does |
|---------|-------------|
| `/pdf:open` | Open a PDF in the interactive viewer |
| `/pdf:annotate` | Walk through the document, propose + apply markup, review together |
| `/pdf:fill-form` | Fill PDF form fields interactively |
| `/pdf:sign` | Place a signature or initials image on the page |

## When to use this vs. just reading a PDF

This plugin is for **interactive, visual workflows** — when you want to
see the document, mark it up, and download an annotated copy.

If you just want Claude to **summarize or extract text** from a PDF,
don't use this plugin. Claude can read PDF files natively and that's
faster for pure ingestion.

## How It Works

This plugin uses a **local MCP server** (`@modelcontextprotocol/server-pdf`)
that runs on your machine via `npx`. No API keys or remote services —
the PDF server starts automatically when the plugin loads.

## Requirements

- Node.js >= 18
- Internet for remote PDFs (arXiv, etc.)

## Supported PDF Sources

- Local files (file paths in your working directory)
- [arXiv](https://arxiv.org), [bioRxiv](https://biorxiv.org),
  [medRxiv](https://medrxiv.org), [chemRxiv](https://chemrxiv.org)
- [Zenodo](https://zenodo.org), [OSF](https://osf.io),
  [HAL Science](https://hal.science), [SSRN](https://ssrn.com)

## Signature Disclaimer

`/pdf:sign` places a **visual** signature image on the page. It is not
a certified or cryptographic digital signature. For legally binding
e-signatures, use a dedicated signing service.
