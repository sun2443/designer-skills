---
name: pdf
description: Interactive PDF viewer for annotating, form-filling, and signing documents. Use when the user wants to SEE a PDF, collaboratively mark it up, fill in form fields, place a signature, or review annotations together. Do NOT use for pure text extraction or summarization — Claude's native Read tool handles PDF ingestion more efficiently.
---

# PDF — Interactive Document Workflows

You have access to a local PDF server that renders documents in a live
viewer and lets you annotate, fill forms, and place signatures with
real-time visual feedback.

## When to use this skill

**Use the PDF viewer when the user wants interactivity:**
- "Show me this contract" / "Open this paper"
- "Highlight the key terms and let me review"
- "Help me fill out this form"
- "Sign this on page 3" / "Add my initials to each page"
- "Stamp this CONFIDENTIAL" / "Mark this as approved"
- "Walk me through this document and annotate the important parts"

**Do NOT use the viewer for pure ingestion:**
- "Summarize this PDF" → use the native Read tool directly
- "What does page 5 say?" → use Read
- "Extract the table from section 3" → use Read

The viewer's value is showing the user the document and collaborating
on markup — not streaming text back to you.

## Tools

### `list_pdfs`
List available local PDFs and allowed remote origins. No arguments.

### `display_pdf`
Open a PDF in the interactive viewer. **Call once per document.**
- `url` — local file path or HTTPS URL
- `page` — initial page (optional, default 1)
- `elicit_form_inputs` — if `true`, prompts the user to fill form
  fields before displaying (use for interactive form-filling)

Returns a `viewUUID` — pass this to every `interact` call. Calling
`display_pdf` again creates a **separate** viewer; interact calls with
the new UUID won't reach the one the user is looking at.

Also returns `formFields` (name, type, page, bounding box) if the PDF
has fillable fields — use these coordinates for signature placement.

### `interact`
All follow-up actions after `display_pdf`. Pass `viewUUID` plus one or
more commands. **Batch multiple commands in one call** via the
`commands` array — they run sequentially. End batches with
`get_screenshot` to verify changes visually.

**Annotation actions:**
- `add_annotations` — add markup (see types below)
- `update_annotations` — modify existing (id + type required)
- `remove_annotations` — delete by id array
- `highlight_text` — auto-find text by query and highlight it
  (preferred over manual rects for text markup)

**Navigation actions:**
- `navigate` (page), `search` (query), `find` (query, silent),
  `search_navigate` (matchIndex), `zoom` (scale 0.5–3.0)

**Extraction actions:**
- `get_text` — extract text from page ranges (max 20 pages). Use for
  reading content to decide what to annotate, NOT for summarization.
- `get_screenshot` — capture a page as PNG (verify your annotations)

**Form action:**
- `fill_form` — fill named fields: `fields: [{name, value}, ...]`

## Annotation Types

All annotations need `id` (unique string), `type`, `page` (1-indexed).
Coordinates are PDF points (1/72 inch), origin **top-left**, Y increases
downward. US Letter is 612×792pt.

| Type | Key properties | Use for |
|------|----------------|---------|
| `highlight` | `rects`, `color?`, `content?` | Mark important text |
| `underline` | `rects`, `color?` | Emphasize terms |
| `strikethrough` | `rects`, `color?` | Mark deletions |
| `note` | `x`, `y`, `content`, `color?` | Sticky-note comments |
| `freetext` | `x`, `y`, `content`, `fontSize?` | Visible text on page |
| `rectangle` | `x`, `y`, `width`, `height`, `color?`, `fillColor?` | Box regions |
| `circle` | `x`, `y`, `width`, `height`, `color?`, `fillColor?` | Circle regions |
| `line` | `x1`, `y1`, `x2`, `y2`, `color?` | Draw lines/arrows |
| `stamp` | `x`, `y`, `label`, `color?`, `rotation?` | APPROVED, DRAFT, CONFIDENTIAL, etc. |
| `image` | `imageUrl`, `x?`, `y?`, `width?`, `height?` | **Signatures, initials**, logos |

**Image annotations** accept a local file path or HTTPS URL (no data:
URIs). Dimensions auto-detected if omitted. Users can also drag & drop
images directly onto the viewer.

## Interactive Workflows

### Collaborative annotation (AI-driven)
1. `display_pdf` to open the document
2. `interact` → `get_text` on relevant page range to understand content
3. Propose a batch of annotations to the user (describe what you'll mark)
4. On approval, `interact` → `add_annotations` + `get_screenshot`
5. Show the user, ask for edits, iterate
6. When done, remind them they can download the annotated PDF from the
   viewer toolbar

### Form filling
1. `display_pdf` with `elicit_form_inputs: true` — the user fills
   fields in a prompt before the viewer opens
2. OR: `display_pdf`, inspect returned `formFields`, ask the user for
   values, then `interact` → `fill_form`
3. `get_screenshot` to confirm

### Signing (visual, not certified)
1. Ask for the signature/initials image path
2. `display_pdf`, check `formFields` for signature-type fields or ask
   which page/position
3. `interact` → `add_annotations` with `type: "image"` at the target
   coordinates
4. `get_screenshot` to confirm placement

**Disclaimer:** This places a visual signature image. It is **not** a
certified or cryptographic digital signature.

## Supported Sources

- Local files (paths under client MCP roots)
- arXiv, bioRxiv, medRxiv, chemRxiv, Zenodo, OSF, HAL Science, SSRN

## Out of Scope

- **Summarization / text extraction** — use native Read instead
- **Certified digital signatures** — image stamping only
- **PDF creation** — this works on existing PDFs only
