---
applyTo: "config/**"
---

This directory holds the **only** files that configure the keyboard. Follow
`AGENTS.md` → "Rules for editing this repo".

- `eyelash_corne.keymap` is the single source of truth for keys. Do not add
  `corne_left.dts` / `corne_right.dts`; the shield DTS comes from the
  `eyelash_corne` module in `west.yml`.
- Bindings are positional per the matrix transform in the keymap header comment
  (left hand `0 1 2 3 4 5 13 14 15 16 17 18 22 23 24 29 30 31`, right hand
  `7 8 9 10 11 12 19 20 21 25 26 27 28 32`, thumbs `33 34 35 36 37 38`). Keep the
  rows visually aligned and give every layer the same number of entries.
- New behaviours/combos go above `keymap { }`; `#binding-cells` (or
  `#sensor-binding-cells` for encoder behaviours) must match the parameters used.
- Reuse `home_mods` via `&hm <MOD> <KEY>` and `thumb_lt` via `&thumb_lt <layer> <key>`
  instead of bare `&mt` / `&lt`. Don't retune their flavour, timings or
  `hold-trigger-key-positions`, and don't put a combo on a chorded home-row mod
  pair — read `user-preference-docs/typing-profile.md` (if present) first.
- `CONFIG_*` belongs in `eyelash_corne.conf`; it applies to both halves. Use
  `&rgb_ug RGB_ON` / `RGB_OFF` — never `&ext_power EP_*`.
- Don't rename the `.keymap` / `.conf` files. Don't bump `west.yml` revisions
  (the ZMK fork is `v0.3-branch+dya` and must match the shield module).
- After editing, update `README.md` layer diagrams and the keymap header comment.
