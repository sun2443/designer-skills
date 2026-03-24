---
description: Fill PDF form fields interactively
---

# Fill Form

Help the user complete a fillable PDF form with live preview.

## Two approaches

### User-driven (recommended for simple forms)

Call `display_pdf` with `elicit_form_inputs: true`. The server detects
form fields and prompts the user to enter values **before** the viewer
opens. The filled PDF is then displayed.

### AI-assisted (for complex forms or when you have context)

1. `display_pdf` (without elicit) — inspect the returned `formFields`
   array (name, type, page, bounding box)
2. For each field, either:
   - Infer the value from conversation context (name, date, email)
   - Ask the user directly
3. `interact` → `fill_form` with `fields: [{name, value}, ...]`
4. `interact` → `get_screenshot` of each page with filled fields
5. Ask the user to confirm before they download

## Example

> **User:** Help me fill out this W-9
>
> *You:* `display_pdf` → see formFields: Name, Business name, Address,
> TIN, Signature, Date
>
> *You:* "I see 6 fields. I can fill Name and Date from what I know —
> what's your TIN and business address?"
>
> *After answers:* `interact` → `fill_form` + `get_screenshot`
>
> *You:* "Here's the filled form. The signature field is still blank —
> want to add your signature image with `/pdf:sign`?"

## Notes

- Signature fields are usually separate from text fields — fill text
  first, then hand off to `/pdf:sign` for the image
- Some forms have checkboxes/radio buttons — `value` is `true`/`false`
  or the selected option string
