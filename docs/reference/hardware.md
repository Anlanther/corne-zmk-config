# Hardware & Pin Reference

## Core

| Item                            | Value                                                           |
| ------------------------------- | --------------------------------------------------------------- |
| MCU                             | nRF52840 (`52840micro` controller board — nice!nano compatible) |
| Firmware                        | ZMK                                                             |
| Split transport                 | BLE (1000 Hz report rate when using a dongle receiver)          |
| Main / central half             | **Left**                                                        |
| Peripheral half                 | Right                                                           |
| Battery (new Corne)             | 1500 mAh, 504060 Li-ion, PH2.0 connector                        |
| Battery (Sofle, for comparison) | 2000 mAh, 505060                                                |

The battery has an on-board protection circuit and stops charging when full. The keyboard runs fine from any phone charger (5 V) via the USB-C port.

## Matrix definitions

The key matrix is defined in the repository's device-tree files:

```text
config/<keyboard>.dtsi          # shared matrix, kscan, sensor (encoder), split config
config/<keyboard>_left.dts      # central: adds BLE, display, split central role
config/<keyboard>_right.dts     # peripheral: adds split peripheral role
```

For the Corne those files are `corne.dtsi`, `corne_left.dts`, `corne_right.dts`; the Sofle equivalents are `sofle.dtsi`, `sofle_left.dts`, … (the manual spells the Sofle ones inconsistently as `solfe_left.dts` — the real file is `sofle_left.dts`).

**If you change the matrix** (add a key, change a row/col pin, change diode direction) you must update the `kscan` node in the `.dtsi` **and** the `keymap` in the same commit, otherwise the build compiles but keys land in the wrong places.

## Spare GPIO

| Half  | Pin     |
| ----- | ------- |
| Left  | `P0.29` |
| Right | `P1.13` |

The Sofle has **no** spare GPIO (space constrained). On the Corne there is a pad for this pin below the controller; expansion pads on the back of the PCB are labelled in silkscreen (power, extra IO).

## Switches & physical parts

| Part                                   | Specification                                       |
| -------------------------------------- | --------------------------------------------------- |
| Power slide switch (new Corne)         | mini `MKS12C01`                                     |
| Power slide switch (old Corne / Sofle) | regular `MKS12C02`                                  |
| Reset press switch (new Corne / Sofle) | `TS24CA`                                            |
| Embedded nuts                          | `M2×2×3.2` (M2×2×3 brass nuts also fit)             |
| PCB mounting screws                    | `M2×4`                                              |
| Switch-plate → PCB screws              | `M2×4` self-tapping                                 |
| Screen cover                           | tempered glass, retained by `M2×2` screws           |
| Feet                                   | `10×2` mm rubber pads                               |
| Switch sockets                         | hot-swap (Kailh-style); Choc sockets on low-profile |

Battery switch direction on the Corne: both halves **slide down = on**. On the low-profile Sofle it is **towards the outside of the keyboard = on**.

## LEDs

| LED              | Meaning                                                                                          |
| ---------------- | ------------------------------------------------------------------------------------------------ |
| Green (charging) | solid = charging · flashing = battery disconnected or power switch off · off = full              |
| Blue (MCU)       | breathing = bootloader + USB attached · fast flash = bootloader, no USB host · off = ZMK running |
| Per-key RGB      | underglow; see [Custom behaviours → RGB power](custom-behaviours.md#rgb-power-saving)            |

Both indicator LEDs are at the USB-C port on the new Corne and Sofle and are only faintly visible.

## Rotary encoder

An EC11-style encoder is wired as a `sensor` binding on the left half:

```dts
sensor-bindings = <&inc_dec_kp C_VOL_UP C_VOL_DN>;
```

(The shipped schematic label is `sinc_dec_kp` in the diagrams; the ZMK behaviour is `&inc_dec_kp`.) Layer 0 rotates volume, other layers scroll the wheel via a separate `&msc`/scroll binding — see [Custom behaviours](custom-behaviours.md#rotary-encoder).

## Screen

Two display variants exist:

- **OLED** (`nice_oled` shield) — older Corne-OLED builds.
- **Nice View** (`nice_view`) — new Corne and dongle builds.

The `.dts` files contain the `&zephyr_io_spi1` / `&nice_view_spi` style display node. Display on/off is controlled by `CONFIG_ZMK_DISPLAY` (see [Config options](config-options.md)), and widgets come from `zmk-nice-oled` / `zmk-nice-view` modules plus custom widget code (`lvgl`). The screen's icons are documented in [End-user behaviour reference](user-reference.md#display-and-receiver-half).
