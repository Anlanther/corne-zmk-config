# Config Options

ZMK Kconfig settings live in `config/corne.conf` (both halves), `config/corne_left.conf` (central only), and `config/corne_right.conf` (peripheral only). Each half is a separate firmware image, so a setting that must be identical must go in `corne.conf` or in **both** side files.

> Rule from the manual: **don't change a parameter you don't understand.** Several settings are chained (e.g. Studio needs a large enough endpoint buffer, and RGB needs its own driver) and one wrong value can fail the build or brick Bluetooth behaviour. Change one at a time and rebuild.

## Common settings

| Setting | Values | Notes |
| --- | --- | --- |
| `CONFIG_ZMK_DISPLAY` | `y` / `n` | Display support. `n` for a Corne with no screen; saves battery and flash. |
| `CONFIG_ZMK_SLEEP` | `y` / `n` | Deep sleep (`&soft_off`). Turning sleep off can reduce macOS reconnection issues. |
| `CONFIG_ZMK_IDLE_SLEEP_TIMEOUT` | ms | How long before the keyboard sleeps when idle. |
| `CONFIG_ZMK_IDLE_TIMEOUT` | ms | How long before the keyboard idles (screen off, lower power). |
| `CONFIG_ZMK_SPLIT_BLE_CENTRAL_PERIPHERALS` | `1` | Number of peripherals; keep `1` for a two-half Corne. |
| `CONFIG_ZMK_POINTING` | `y` | Enables `&mmv` / `&mkp` mouse behaviours (required for the joystick layers). |
| `CONFIG_BT_CTLR_TX_PWR_PLUS_8` | `y` | Increases BLE transmit power (helps marginal links, costs battery). |
| `CONFIG_ZMK_SETTINGS_SAVE_DEBOUNCE` | ms | Debounce for saving settings (profiles, etc.) to flash. |

### Display off (headless Corne)

```conf
# corne.conf
CONFIG_ZMK_DISPLAY=n
```

You can use the ink-screen repository and firmware for a headless Corne this way, or simply flash it with the display disabled and ignore the screen widgets.

### Sleep tuning for macOS

```conf
# corne.conf
CONFIG_ZMK_SLEEP=n
```

Disabling sleep avoids many "keyboard won't reconnect after idle" complaints from Mac users; the trade-off is higher idle current draw — compensate with a longer `CONFIG_ZMK_IDLE_SLEEP_TIMEOUT` or the power switch.

## RGB / underglow

| Setting | Meaning |
| --- | --- |
| `CONFIG_ZMK_RGB_UNDERGLOW` | `y` to enable `&rgb_ug` keycodes |
| `CONFIG_ZMK_RGB_UNDERGLOW_AUTO_OFF_IDLE` | Turn LEDs off when idle |
| `CONFIG_ZMK_RGB_UNDERGLOW_AUTO_OFF_USB` | Turn LEDs off while on USB |
| `CONFIG_ZMK_RGB_UNDERGLOW_EXT_POWER` | Tie the LED rail to the `ext_power` behaviour |

The shipped builds auto-cut the LED rail when RGB is off, so `CONFIG_ZMK_RGB_UNDERGLOW_EXT_POWER=y` plus `RGB_OFF` is the battery-saving combination. See [custom behaviours → RGB power saving](custom-behaviours.md#rgb-power-saving).

## Bluetooth

| Setting | Meaning |
| --- | --- |
| `CONFIG_BT_MAX_CONN` / `CONFIG_BT_MAX_PAIRED` | Max simultaneous connections / stored pairings (the manual advertises 4–5 profiles). |
| `CONFIG_ZMK_BLE_EXPERIMENTAL_CONN` | Tuning knob for connection interval / latency; leave alone unless you know why. |
| `CONFIG_ZMK_SPLIT_BLE_CENTRAL_BATTERY_LEVEL_FETCHING` | Show the peripheral's battery on the central's screen. |

## Studio (DYA STUDIO / ZMK Studio)

| Setting | Meaning |
| --- | --- |
| `CONFIG_ZMK_STUDIO` | `y` to expose the Studio endpoint over USB/BLE |
| `CONFIG_ZMK_STUDIO_LOCKING` | Lock/unlock protection before edits are applied |
| `CONFIG_ZMK_RUNTIME_KEYMAP` | Store the modified keymap flash/ RAM (Studio needs this) |

DYA STUDIO firmware for this board is built with Studio enabled; if you compile a plain ZMK repo, Studio will not be available.

## Dongle / receiver builds

The dongle shield enables the split central role on the USB receiver and re-runs the BLE profile logic there:

- Keymap changes go on the **receiver** image (`central_dongle`).
- Per-half settings (e.g. display options) still live in the left/right `defconfig`s.
- `CONFIG_ZMK_SPLIT_ROLE_CENTRAL` must be `y` only in the dongle image, never in the half images.

## Defconfig vs conf

- `boards/.../*_defconfig` — board/shield-level defaults, used by the shield definition.
- `config/corne.conf` — your overrides; these win over defconfig.

Prefer `corne.conf` over editing `.dtsi` for anything that has a Kconfig option; DTS changes affect the hardware description and can break the matrix if you mistype a pin.
