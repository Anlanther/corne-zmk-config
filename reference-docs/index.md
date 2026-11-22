# Corne Developer Documentation

Technical reference for anyone editing the **Corne** firmware or configuration repository (keymap, behaviours, config, build/flashing). Derived from the shipped manuals in [`../docs/`](../docs/index.md) — the user-facing docs are there; this folder is the "I'm going to change something" version.

> **Scope:** Corne (MX and low-profile, with and without dongle). Sofle is nearly identical at the ZMK level; differences are called out where they matter.

## Contents

| Doc | Read it when you want to… |
| --- | --- |
| [Hardware & pin reference](hardware.md) | Know the MCU, battery, matrix files, spare GPIO, switch/screw part numbers, LEDs. |
| [Repository layout & build](build-and-flash.md) | Understand `config/`, which file to edit, how GitHub Actions builds, and how to flash safely. |
| [Layer & keymap reference](layers.md) | See the default 4-layer layout as code-equivalent tables. |
| [Custom behaviours](custom-behaviours.md) | Understand `&td0` tap-dance, `&lt` thumb keys, encoder bindings, mouse/joystick layers. |
| [Config options](config-options.md) | Toggle display, sleep, RGB/power, and other `CONFIG_*` settings. |
| [Troubleshooting for developers](troubleshooting.md) | Fix build failures, wrong-firmware symptoms, pairing problems, and the known footguns. |
| [Upstream references](references.md) | Links to ZMK docs and the upstream repos. |

## Quick facts

- **MCU:** nice!nano-class board (`52840micro`, nRF52840). 4.2 V Li-ion battery.
- **Battery:** new Corne = 1500 mAh 504060, connector PH2.0.
- **Main half:** **left** (central). Right is the peripheral. All keycodes pass through the left half.
- **Firmware repositories** (pick the one matching your hardware):
  - Ink-screen Corne: <https://github.com/a741725193/zmk-new_corne>
  - OLED Corne: <https://github.com/a741725193/zmk-corne-oled>
  - Corne + dongle: <https://github.com/a741725193/zmk-corne-dongle> (OLED branch for OLED-dongle)
  - Offset (related board): <https://github.com/a741725193/zmk-config-offsetkey>
- **Layer count:** 4 (`default_layer`, `lower_layer`, `raise_layer`, `layer_3`).
- **Only two halves are ever in one firmware bundle:** `left` and `right`, plus a `settings_reset` build.

> Verify every claim here against the actual `.keymap` / `.conf` / `.dtsi` in the repository you forked — the tables in this folder are reconstructed from the shipped layer diagrams and manuals, not read from source.
