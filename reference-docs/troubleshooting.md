# Troubleshooting for Developers

Split into **build problems** (your change), **runtime problems** (firmware behaviour), and **hardware problems** (symptom → likely part). For end-user diagnostics in more detail see [../docs/split-keyboard-manual.md](../docs/split-keyboard-manual.md#troubleshooting).

## Build problems

| Symptom | Cause | Fix |
| --- | --- | --- |
| Red × in Actions | Syntax error in `.keymap` — usually a missing comma between bindings, an extra `&`, or a `#binding-cells` mismatch | Click the failed run → the log points at the line. Revert the commit, or re-fork. |
| `undefined behavior reference &xyz` | Behaviour used in the keymap is not defined **above** the `keymap` node | Move the `behaviors { … }` block before `keymap { … }`. |
| `unknown keycode` / `undefined identifier` | Keycode typo, or a keycode not defined in ZMK's header | Check the exact name at <https://zmk.dev/docs/codes>. |
| Build succeeds but the firmware name changed | You edited `build.yaml` or renamed a config file | Rename back; the shield name in `build.yaml` must match the `.keymap` base name. |
| `CONFIG_… was assigned … but got …` | A Kconfig dependency isn't met (e.g. RGB without the LED driver, Studio without runtime keymap) | Remove your setting or enable the dependency too. |
| Repo doesn't build at all after forking | Forked a repo with a stale `west.yml` or a broken workflow permission | Re-fork from the shipped repo; don't carry over `west.yml` changes. |

## Runtime problems

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| Keys in the **wrong places** | Wrong firmware for the half, or `build.yaml` mixed left/right, or you edited `kscan` without the keymap | Flash `left` on left, `right` on right. Compare against `corne_left.dts`. |
| Keys register but on the **wrong layer** | Layer numbers in `&mo`/`&lt` don't match the `keymap` node order | `&mo N` is **positional**: 0 = first node, 1 = second, etc. Re-check after inserting a layer. |
| Keys do nothing at all | `&trans` on layer 0 (nothing beneath it), or `&none` | Map to a real binding. |
| Space becomes layer 3 while typing | `&lt` hold-tap mis-detects holds | Increase `require-prior-idle-ms`, or use `tap-preferred`. |
| Bluetooth won't connect after re-flash | Stale pairing between halves or host | Flash `settings_reset` to **both** halves, then the normal firmware, then re-pair the host. |
| Right half is dead / shows × / Wi-Fi icon but no input | Left/right pairing lost, or wrong right firmware | Reset both halves; press reset on the right once — if it comes back, it's a link loss, not a dead MCU. |
| Battery dies in a day | Underglow left on, or a loop in a behaviour, or `CONFIG_ZMK_POINTING` polling a sensor aggressively | Turn RGB off; check `&mmv` isn't bound on layer 0 continuously; measure idle current. |
| Keyboard only works over USB | Bluetooth antenna/range, or `OUT_USB`/`OUT_BLE` left on a layer, or `settings_reset` needed | Check `&out` keys aren't stuck; re-pair; check the BLE TX power setting. |

## "It works but feels wrong"

- **Typing lag** — tap-dance (`&td0`) and hold-tap (`&lt`) add timing delays. If it's unacceptable, swap `&td0` for `&kp LSHFT` on layer 0 and keep Caps Lock at the end of a layer.
- **Same key twice** — you edited both `corne_left.dts` and `corne_right.dts` while the keymap lives only in `corne.keymap` (which is built into both images). The keymap is the single source of truth.
- **Encoder jerks in the wrong direction** — swap the order of the two keycodes in `&inc_dec_kp A B`.
- **Joystick cursor drifts** — expected on these sensors; you can raise `CONFIG_…_POINTING_…` dead-zone values or keep the joystick only on mouse layers.

## Hardware symptoms → suspect part

| Symptom | Suspect |
| --- | --- |
| Works on USB, never charges, green LED flashes forever | Power slide switch (`MKS12C01` new / `MKS12C02` old) — a shorted/broken switch; bridge the pads as a stopgap. |
| One key never registers | Choc/MX switch leg bent or not seated in the hot-swap socket. |
| Half dies when unplugged | Battery disconnected / polarity reversed, or the power switch is off. |
| No UF2 drive ever appears | Charge-only USB cable, or reset double-click too slow. |
| Left/right drops at >1 m | Expected (~0.6–0.8 m); use a dongle for desk placement, or raise BLE TX power. |
| Right half works but PC ignores it | Correct — the right half has **no host connection**; it only reaches the PC through the left half. |

## How to get evidence

- **Screen**: layer name + profile icons tell you which layer and which BLE profile the central half thinks it's on ([../docs/display-instructions.md](../docs/display-instructions.md)).
- **Logs**: build a debug image (`CONFIG_ZMK_LOG_LEVEL_DBG=y`) and watch the USB serial console (`screen /dev/ttyACM0 115200` on macOS/Linux).
- **Battery / connection state**: `&sys_reset` re-initialises the display and BLE stack — a good "did the peripheral even boot" check.

## Escalation

- Hardware faults: contact <380465425@qq.com>.
- Firmware questions: the ZMK Discord / GitHub discussions — bring the exact repo URL and the failing build log.
- If a change can't be made to build, re-fork the shipped repo; that resets everything to a known-good state.
