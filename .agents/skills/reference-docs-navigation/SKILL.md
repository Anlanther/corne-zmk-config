---
name: reference-docs-navigation
description: Look up the shipped/default Eyelash Corne ZMK behaviour and general ZMK guidance in this repo's reference-docs/ folder. Use when changing layers or keycodes, hold-taps, combos, encoder bindings, CONFIG_* options, build.yaml/west.yml, artifacts and flashing, hardware/matrix/pins, or diagnosing a build or "wrong key/layer" problem — and to check whether a reference doc disagrees with config/.
---

# Navigating `reference-docs/`

`reference-docs/` documents the **shipped/default** Corne firmware and general ZMK
practice. It is **reference, not truth for this repo**. Whenever a doc and
`config/eyelash_corne.keymap` / `config/eyelash_corne.conf` disagree, the `config/`
files win. Never edit `reference-docs/` to match the keymap.

## 1. Route the task — read at most 1–2 docs

| Task                                                                                          | Read                                  |
| --------------------------------------------------------------------------------------------- | ------------------------------------- |
| Layers, key positions, keycodes, layer indexes, editing cheat-sheet                           | `reference-docs/layers.md`            |
| Hold-taps (`&lt`, `&mt`, tap-dance), mod-morph, combos, encoder, RGB power, adding behaviours | `reference-docs/custom-behaviours.md` |
| `CONFIG_*` options (display, sleep, RGB, BLE, Studio, pointing)                               | `reference-docs/config-options.md`    |
| `build.yaml`, `west.yml`, CI, artifact names, flashing / `settings_reset`                     | `reference-docs/build-and-flash.md`   |
| Build failure or runtime symptom (wrong keys, wrong layer, dead half, battery)                | `reference-docs/troubleshooting.md`   |
| Keycode/behaviour names, upstream repos and zmk.dev links                                     | `reference-docs/references.md`        |
| MCU, battery, matrix/`kscan`, spare GPIO, encoder, display hardware                           | `reference-docs/hardware.md`          |
| Unsure / need the whole map                                                                   | `reference-docs/index.md`             |

Fastest path for a keymap change: `config/eyelash_corne.keymap` (current state) →
`reference-docs/layers.md` + `custom-behaviours.md` (syntax only). You do **not**
need `hardware.md`, `config-options.md` or `build-and-flash.md` for a key swap.

## 2. Apply the divergence table before trusting any doc

These are the known mismatches between the docs (default firmware) and this repo:

| Topic             | Docs say                                                     | This repo actually does                                                                                                                                                                      |
| ----------------- | ------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Layers            | 4: `default_layer`, `lower_layer`, `raise_layer`, `layer_3`  | 3: `default_layer` (Base), `utility_layer` (Utility), `number_layer` (Number)                                                                                                                |
| `&td0` tap-dance  | top-left Shift/Caps tap-dance                                | not used; `&hm` home-mods + `&thumb_lt` layer-taps, and `&thumb_lt 2 LA(ESC)` (tap `Opt+Esc`, hold Number) on the rightmost right thumb                                                      |
| Thumb keys        | `&mo 1`, `&mo 2`, `&lt 3 …`, `&kp LGUI`, `&kp RALT`          | `&thumb_lt 2 ESC`, `&thumb_lt 1 RET`, `&hm LGUI DEL`, `&kp BSPC`, `&thumb_lt_own 1 SPACE`, `&thumb_lt 2 LA(ESC)`                                                                             |
| Thumb stick       | joystick sensor with `&mmv` / `&mkp`                         | 5 plain matrix keys in a cross (up `6`, left `19`, click `20`, right `21`, down `35`) — Mission Control, Show Desktop, Launchpad, Caps toggle, Spotlight on Base only; transparent elsewhere |
| Encoder           | `&inc_dec_kp C_VOL_UP C_VOL_DN`, per-layer scroll            | `&rsr_vol` on every layer (`sensor-bindings = <&rsr_vol &rsr_trans>`)                                                                                                                        |
| Soft off          | `&soft_off` key / `&ext_power EP_*`                          | combo Q+S+Z (`key-positions = <1 15 29>`) + `CONFIG_ZMK_PM_SOFT_OFF=y`                                                                                                                       |
| Mouse layer       | `&mmv`/`&mkp` on upper layers                                | `CONFIG_ZMK_POINTING=y` set, but no pointing bindings                                                                                                                                        |
| File names        | `corne.keymap`, `corne.conf`, `corne.dtsi`, `corne_left.dts` | `eyelash_corne.keymap` / `.conf`; shield DTS lives in the module                                                                                                                             |
| Number layer      | `NUMBER` layer holds F-keys/BT/RGB                           | Utility holds `F2 F3 C_PREV C_PP C_NEXT`; Number holds only `N0`–`N9`                                                                                                                        |
| Sofle sections    | mixed in for comparison                                      | ignore entirely                                                                                                                                                                              |
| `../docs/…` links | point to shipped manuals                                     | that folder is not in this repo — fall back to zmk.dev                                                                                                                                       |

## 3. Map a doc's syntax onto this repo's behaviours

- `&mt <MOD> <KEY>` → prefer `&hm <MOD> <KEY>` (the repo's `home_mods` hold-tap,
  `balanced` + `hold-trigger-on-release`, hold only on right-hand/thumb keys — see
  `user-preference-docs/typing-profile.md` if present). Never define the alias `&hm` twice.
- `&lt <layer> <KEY>` → prefer `&thumb_lt <layer> <KEY>` for thumb keys
  (`hold-trigger-key-positions = <RIGHT_HAND>`).
- `&mo N` for a thumb → `&thumb_lt N <tap-key>` keeps the tap function.
- `&inc_dec_kp …` → this repo uses `zmk,behavior-runtime-sensor-rotate`
  (`&rsr_vol`, `&rsr_trans`); a new rotate binding needs
  `#sensor-binding-cells = <0>` plus `cw-binding` / `ccw-binding`.
- `&ext_power EP_ON/OFF` → do not re-add; use `&rgb_ug RGB_ON` / `RGB_OFF`.

## 4. Useful extracts worth quoting back to the user

- **Artifact → half**: `eyelash_corne_studio_left` → left, `eyelash_corne_right` →
  right, `settings_reset` → either half to re-pair.
- **Flashing**: data USB cable, double-tap reset < 0.5 s, drag one `.uf2` onto
  `nicenano`, never delete files.
- **Wrong layer** after editing: layer indexes are **positional** in the `keymap { }`
  node — recount after inserting/removing a layer node.
- **Space becomes a layer mid-word**: hold-tap mis-detection — raise
  `require-prior-idle-ms` (the repo's own hold-taps are already at 175 ms with
  `balanced` + `hold-trigger-on-release`; don't switch them to `tap-preferred`, see
  `user-preference-docs/typing-profile.md` if present).
- **Local build** (`build-and-flash.md`) uses `corne_*` shields — substitute
  `eyelash_corne_left` / `eyelash_corne_right` and board `nice_nano_v2`, and only
  after asking the user.

## 5. After looking something up

Confirm the syntax you plan to write actually exists in
`config/eyelash_corne.keymap` (behaviours above `keymap { }`, matching
`#binding-cells`), then follow `AGENTS.md` → "Rules for editing this repo" and
"Before you finish".
