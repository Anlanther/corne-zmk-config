# Agent guide — Eyelash Corne ZMK config

Shared instructions for **any** coding agent (Zed's agent, GitHub Copilot, etc.).
Claude/Copilot tooling reads this same file via the `CLAUDE.md` / `.github/copilot-instructions.md` pointers.

## What this repo is

A ZMK **user config** repo (not ZMK itself) for an **Eyelash Peripherals Corne**
(`eyelash_corne` shield) — 48 keys, 5-row matrix, nice!nano v2 ×2, nice!view displays.

> **Not a foostan Corne.** `corne_left` / `corne_right` shields are wrong here.
> Always use `eyelash_corne_left` / `eyelash_corne_right`.

## Ground truth (read before editing)

| File                          | What it is                                                                                                      |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------- |
| `config/eyelash_corne.keymap` | **The** keymap. 3 layers: `default_layer` (Base), `utility_layer` (Utility), `number_layer` (Number).           |
| `config/eyelash_corne.conf`   | Kconfig overrides for both halves.                                                                              |
| `config/west.yml`             | Pulls the `eyelash_corne` shield module, a DYA-patched ZMK fork, and 5 helper modules.                          |
| `build.yaml`                  | Only build targets: `eyelash_corne_right nice_view`, `eyelash_corne_left nice_view` (studio), `settings_reset`. |
| `.github/workflows/build.yml` | Reusable upstream ZMK build workflow. Builds on every push.                                                     |
| `README.md`                   | Human-facing layer diagrams — keep in sync with the keymap.                                                     |

## How to use `reference-docs/`

`reference-docs/` describes the **shipped/default** Corne config and general ZMK
guidance. It is **reference, not truth for this repo** — several files don't match
the custom build. Do not treat it as the current keymap.

Pick the file by task, don't read them all:

| You are about to…                                              | Read first                                                                |
| -------------------------------------------------------------- | ------------------------------------------------------------------------- |
| Change layers, keycodes, hold-taps, combos, encoder bindings   | `reference-docs/layers.md`, `reference-docs/custom-behaviours.md`         |
| Add/adjust `CONFIG_*` options                                  | `reference-docs/config-options.md`                                        |
| Touch `build.yaml`, `west.yml`, or explain artifacts/flashing  | `reference-docs/build-and-flash.md`                                       |
| Diagnose a build failure or "wrong keys / wrong layer" symptom | `reference-docs/troubleshooting.md`                                       |
| Look up a keycode or upstream behaviour                        | `reference-docs/references.md` (then zmk.dev), `reference-docs/layers.md` |
| Touch the matrix, pins, encoder, display hardware              | `reference-docs/hardware.md`                                              |
| Unsure which doc applies                                       | `reference-docs/index.md` (has the full map)                              |

### Known divergences: reference-docs vs this repo

Verify against the `config/` files; the docs describe the **default firmware**.

| Topic            | reference-docs say                                                             | This repo actually does                                                                                               |
| ---------------- | ------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------- |
| Layers           | 4 layers: `default_layer`, `lower_layer`, `raise_layer`, `layer_3` (`FW`/`FN`) | **3 layers**: `default_layer`, `utility_layer`, `number_layer`. No `layer_3`.                                         |
| Display names    | `QWERTY` / `NUMBER` / `SYMBOL` / `FN`                                          | `Base` / `Utility` / `Number`                                                                                         |
| Tap-dance `&td0` | Top-left key tap-dance (Shift / Caps)                                          | **Not used.** Uses `&hm` home-mod hold-taps (`home_mods`) and `&thumb_lt` (local hold-tap).                           |
| Mouse / joystick | Center cluster is a pointer sensor, `&mmv`/`&mkp`                              | Thumb stick is **4 matrix keys** (arrows). Only `&mmv`-free; `CONFIG_ZMK_POINTING=y` is set but no pointing bindings. |
| Files            | `corne.keymap`, `corne.conf`, `corne.dtsi`, `corne_left.dts` …                 | `eyelash_corne.keymap`, `eyelash_corne.conf`. Shield DTS lives in the `eyelash_corne` module, not in this repo.       |
| `settings_reset` | In shipped bundle                                                              | Present in `build.yaml` (target available if needed).                                                                 |
| Encoder          | `&inc_dec_kp` with per-layer `sensor-bindings`                                 | `&rsr_vol` / `&rsr_trans` = `zmk,behavior-runtime-sensor-rotate` (volume on every layer).                             |
| Soft off         | `&soft_off` on a key, plus `EP_*` keys                                         | Combo `Q+S+Z` + `CONFIG_ZMK_PM_SOFT_OFF=y`; no `EP_*` keys.                                                           |
| Sofle notes      | Interspersed for comparison                                                    | Not applicable — ignore Sofle sections.                                                                               |

`reference-docs/index.md` and `build-and-flash.md` also link to `../docs/…` — that
folder is **not in this repo**. If a link is dead, fall back to zmk.dev.

## Working style (owner preference)

1. **Never estimate effort, time, or complexity.** Time estimates are noise here —
   the agent works in seconds. Report what is needed and what was verified, not
   "this will take a couple of hours".
2. **Always implement the best solution, not a shortcut.** Don't offer a
   degrade-to-nothing fallback as the recommendation, don't half-build a feature,
   and don't leave TODO stubs wired into the build. If the correct solution is
   large, do the large correct solution; if the hardware makes it impossible,
   show the evidence from the devicetree/hardware files rather than guessing.
3. **Don't ask permission for the recommended path.** Present the plan, then
   execute it end to end and validate.
4. **Write minimal comments.** Comment only non-obvious intent, constraints or
   workarounds. No restating code, no file-header essays, no changelog
   commentary in the source. If a comment explains _why_, keep it to one line.

## Rules for editing this repo

1. **The keymap is the single source of truth.** Edit `config/eyelash_corne.keymap`
   only. Do **not** edit `corne_left.dts` / `corne_right.dts` equivalents for key
   changes — the shield's DTS comes from the upstream module via `west.yml`.
2. **Binding order matters.** Bindings are positional per the shield's matrix
   transform (commented at the top of the keymap: left hand `0 1 2 3 4 5 13 14 15 16 17 18 22 23 24 29 30 31`,
   right hand `7 8 9 10 11 12 19 20 21 25 26 27 28 32`, thumbs `33 34 35 36 37 38`).
   Keep the row layout in the file visually aligned — it is how humans read the map.
3. **Every layer must have the same number of `bindings` entries**, matching the
   matrix. `&trans` inherits from the layer below; `&none` is dead.
4. **Behaviours, combos and macros go before `keymap { … }`** in the same file.
   `#binding-cells` must equal the number of parameters passed.
5. **Layer indexes in `&mo` / `&thumb_lt N` are positional** (0 = first node in
   `keymap { }`). Recheck them after adding/removing/reordering a layer.
6. **`&mo` vs `&thumb_lt`**: this repo defines local hold-tap behaviours
   (`home_mods`, `thumb_lt`) with `tap-preferred` + `hold-trigger-key-positions`.
   Prefer reusing them over bare `&lt` / `&mt`, and note that `home_mods` maps to
   `&kp` on hold, so use `&hm <MOD> <KEY>` (e.g. `&hm LSHFT A`).
7. **`CONFIG_*` changes go in `config/eyelash_corne.conf`**, not into shield DTS.
   The conf applies to both halves; split-only settings need the shield's per-half conf.
8. **Don't rename** `eyelash_corne.keymap` / `eyelash_corne.conf`; the name must
   match the shield. Don't switch `build.yaml` to `corne_*` shields.
9. **Never re-add `&ext_power EP_*`**. Newer firmware ties LED power to underglow;
   use `RGB_OFF` / `RGB_ON`. See `reference-docs/custom-behaviours.md`.
10. **Mirror meaningful layout changes into `README.md` diagrams** and keep the
    header comment block of the keymap in sync with the bindings.
11. **Don't bump `west.yml` revisions** unless the user asks — the ZMK fork is a
    DYA-Studio-patched `v0.3-branch+dya` and must match the shield module.
12. **`CONFIG_ZMK_USB_LOGGING=y` does not build on its own.** It selects Zephyr's
    `USB_UART_CONSOLE`, which needs a `zephyr,console` chosen node; neither the
    nice!nano board layer nor the eyelash_corne shield declares one. Either point
    the console at a CDC-ACM UART (see `config/eyelash_corne_left.overlay`) or
    leave logging off. It also cannot share the USB device with the
    `studio-rpc-usb-uart` snippet — one of them has to go.
13. **ZMK only auto-applies a fixed overlay-name set from `config/`**:
    `<shield>_<board>.overlay`, `<shield>.overlay`, `<board>.overlay`,
    `default.overlay` (and the same for `.conf`). An arbitrarily named
    `config/foo.overlay` is silently ignored — so name debug overlays
    `<shield>.overlay` and scope them by shield.

## Build / flash (for context, not to run)

Firmware is built by GitHub Actions on push. Artifacts:

| Artifact                    | Flash to                            |
| --------------------------- | ----------------------------------- |
| `eyelash_corne_right`       | right half                          |
| `eyelash_corne_studio_left` | left half (central, Studio enabled) |
| `settings_reset`            | either half, only to re-pair        |

Keymap-only changes need **only the left half** re-flashed (zephyr settings do not
carry the keymap; the keymap is compiled into each image, so flash both if both
changed — see `reference-docs/build-and-flash.md`).

## Before you finish

- Re-read the edited layer and count `bindings` entries against the header comment.
- Check that any `&behaviour` you added is defined above `keymap { }` and its
  `#binding-cells` match.
- If you changed a layer's intent, update `README.md` and the keymap header comment.
- Don't attempt `west build` locally unless the user asks — the toolchain and
  module revisions are pinned by CI.
