---
name: quote-a-part
description: Price a CNC machined, turned, laser-cut or 3D-printed part with the QuotedPart tools. Use when someone asks what a part costs to make, wants a quote or price breakdown for a STEP/IGES model or PDF drawing, or compares materials, quantities or shops for a part.
---

# Quote a part with QuotedPart

QuotedPart prices parts the way a job shop does, from the 3D model and/or the PDF drawing:
material, machine time per operation, setups, programming, finishing, extra charges and
markup, at several quantities. The tools come from the `quotedpart` MCP server.

## Steps

1. **Get the part.** Ask for a STEP/IGES model (.step, .stp, .iges, .igs) or a PDF drawing,
   or both. Pass a public link as `file_url`. For a file on the person's computer: when
   `estimate_price` takes `file_path` (a local server), pass the path; otherwise call
   `create_upload`, PUT the raw bytes to `upload_url` (for example
   `curl -sS --upload-file "<file>" "<upload_url>"`), then pass `upload_id`. Use
   `file_base64` only for a small file when you cannot run a command. Always give `filename`
   with its extension. A drawing that belongs to a model goes in the `drawing_*` fields of
   the same call (its own `create_upload` for `drawing_upload_id`).
2. **Material.** If the person named one, pass it as `material` (an id or a plain name such
   as "6061" or "304 stainless"). If they want options, call `list_materials` (filter by
   `process` when known) and offer the closest two or three. If they named none, leave it
   out: the server uses the drawing's material or the shop default.
3. **Quantity.** Pass the quantity they gave. If none, leave it out and show the price
   breaks the tool returns, so the setup cost spread over the lot is visible.
4. **Shop.** Call `list_shops` only when they mention a region, currency or shop, or want to
   compare. Otherwise the default shop is used.
5. **Estimate.** Call `estimate_price`. If the result has `status: "reading"` (a drawing is
   still being read), call `get_quote` with the `quote_id` a little later.
6. **Change it.** To try another material, quantity or process on the same part, call
   `update_estimate` with the `quote_id`. Do not upload the file again.
7. **Report.** Lead with the unit price and line total at the selected quantity, in the
   returned `currency`. Then the cost lines of that break, the other quantities as a short
   table, lead time, and any `checks` or `notes` the tool returned. End with `quote_url`:
   it opens the quote on quotedpart.com, where the person can save it, adjust it and send it.

## Rules

- The price uses QuotedPart's standard rates for the chosen shop, not a binding quote from
  a shop. Say so once.
- Report prices exactly as returned. Do not round, convert currency or recompute them.
- When a part comes back `needs_input` or `failed`, or the tool returns an error, pass its
  message on in one line and say what would fix it (for example re-exporting the STEP file).
- Upload only files the person gave you for pricing.
