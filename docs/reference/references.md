# Upstream References

## Firmware repositories (choose to match your hardware)

| Keyboard / variant         | Repository                                                                             |
| -------------------------- | -------------------------------------------------------------------------------------- |
| Eyelash-corne (ink screen) | <https://github.com/a741725193/zmk-new_corne>                                          |
| Corne-OLED                 | <https://github.com/a741725193/zmk-corne-oled>                                         |
| Corne + dongle             | <https://github.com/a741725193/zmk-corne-dongle>                                       |
| Corne + dongle, OLED       | <https://github.com/tokyo2006/zmk-corne-dongle> (branch `OLED`)                        |
| Offset (related board)     | <https://github.com/a741725193/zmk-config-offsetkey>                                   |
| Sofle                      | <https://github.com/a741725193/zmk-sofle>                                              |
| Sofle-OLED                 | <https://github.com/a741725193/zmk-sofle-oled>                                         |
| Sofle + dongle             | <https://github.com/a741725193/zmk-sofle-dongle> (branch `OLED` for OLED-sofle-dongle) |

## ZMK documentation

| Topic                                                                                  | Link                                            |
| -------------------------------------------------------------------------------------- | ----------------------------------------------- |
| Start / getting the toolchain                                                          | <https://zmk.dev/docs>                          |
| Keymap basics & layer behaviours                                                       | <https://zmk.dev/docs/keymaps>                  |
| Behaviours list (`&kp`, `&mo`, `&lt`, `&mt`, `&bt`, `&rgb_ug`, tap-dance, mod-morph …) | <https://zmk.dev/docs/keymaps/behaviors>        |
| Keycodes (`A`–`Z`, `EXCL`, `AT`, `C_VOL_UP`, `SCRL_UP` …)                              | <https://zmk.dev/docs/codes>                    |
| Keyboard/keypad keycodes (graphical list)                                              | <https://zmk.dev/docs/codes/keyboard-keypad>    |
| Config / Kconfig settings                                                              | <https://zmk.dev/docs/config>                   |
| Split keyboards                                                                        | <https://zmk.dev/docs/features/split-keyboards> |
| Displays                                                                               | <https://zmk.dev/docs/features/displays>        |
| Underglow / RGB                                                                        | <https://zmk.dev/docs/features/underglow>       |
| ZMK Studio                                                                             | <https://zmk.dev/docs/features/studio>          |

## Third-party tools

| Tool                                              | Link                                                   |
| ------------------------------------------------- | ------------------------------------------------------ |
| DYA STUDIO web re-keying                          | <https://studio.dya.cormoran.works/>                   |
| Keymap Editor (graphical, GitHub-backed)          | <https://nickcoutsos.github.io/keymap-editor/>         |
| Display animation module referenced by the manual | <https://github.com/GPeye/urchin-peripheral-animation> |

## Documentation in this folder

- [Index](index.md) — overview and quick facts.
- [Hardware & pin reference](hardware.md)
- [Repository layout & build](build-and-flash.md)
- [Layer & keymap reference](layers.md)
- [Custom behaviours](custom-behaviours.md)
- [Config options](config-options.md)
- [Troubleshooting for developers](troubleshooting.md)
- [End-user behaviour reference](user-reference.md)
- [Keymap diagrams (transcribed)](keymap-diagrams.md)
