---
applyTo: "reference-docs/**"
---

`reference-docs/` documents the **shipped/default** Corne firmware and general ZMK
guidance. It is reference material, **not** this keyboard's current config.

- When you need guidance while editing the keymap or config, use the routing table
  in `AGENTS.md` → "How to use `reference-docs/`". Open one or two docs, not all.
- Never claim, copy, or "fix" content from these docs as if it were the repo's
  current layout. The 3 layers (`Base` / `Utility` / `Number`), `&hm` /
  `&thumb_lt` hold-taps, `&rsr_vol` encoder and the Q+S+Z soft-off combo in
  `config/eyelash_corne.keymap` are authoritative.
- `../docs/…` links inside these files are dead (that folder is not in this repo).
  If more detail is needed, suggest the matching zmk.dev page from
  `reference-docs/references.md`.
- Only change these files when the user explicitly asks for doc updates; prefer
  correcting divergence notes in `AGENTS.md` over rewriting the reference text.
