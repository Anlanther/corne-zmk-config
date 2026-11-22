# Eyelash Corne ZMK Config

ZMK firmware config for an **Eyelash Peripherals Corne** (`eyelash_corne` shield),
built on nice!nano v2 with a nice!view display on each half.

> This is **not** a foostan Corne. It is a 48-key, 5-row matrix and will not
> work with standard `corne` firmware.

## Hardware

| Part           | Detail                                                                                     |
| -------------- | ------------------------------------------------------------------------------------------ |
| Board          | `nice_nano_v2` ×2                                                                          |
| Shield         | `eyelash_corne_left` / `eyelash_corne_right` (from a741725193/zmk-new_corne)               |
| Display        | nice!view on both halves                                                                   |
| Thumb scroller | EC11 encoder, **left half** — bound to volume (CCW down, CW up)                            |
| Thumb stick    | 4 matrix keys (`UP` `LEFT` `RIGHT` `DOWN`) — arrow keys, not a pointer; centre key unbound |

## Layout

48 keys over 5 rows, split across two halves. The thumb cluster has 3 keys
per side; the stick sits between the halves in the upper-middle.

Legend: `key` = tap, `key / hold` where a key has two functions.
`-` = transparent placeholder (inherits from the layer below),
`(none)` = no binding at all.

### Layer 0 — Base

```text
LEFT HALF                                            RIGHT HALF
+-------+-------+-------+-------+-------+-------++-------+-------+-------+-------+-------+-------+
| TAB   | Q     | W     | E     | R     | T     || Y     | U     | I     | O     | P     | \     |
+-------+-------+-------+-------+-------+-------++-------+-------+-------+-------+-------+-------+
| GRAVE | A     | S     | D     | F     | G     || H     | J     | K     | L     | SEMI  | SQT   |
|       | Shift | Ctrl  | Alt   | Cmd   |       ||       | Cmd   | Alt   | Ctrl  | Shift |       |
+-------+-------+-------+-------+-------+-------++-------+-------+-------+-------+-------+-------+
| Shift | Z     | X     | C     | V     | B     || N     | M     | ,     | .     | /     | ESC   |
+-------+-------+-------+-------+-------+-------++-------+-------+-------+-------+-------+-------+

THUMB KEYS
Left:  ESC (hold: Number) | ENT (hold: Utility) | DEL (hold: Cmd)
Right: BKSP               | SPC (hold: Utility) | CAPS (hold: Number)

THUMB STICK
          UP
LEFT - (none) - RIGHT
         DOWN
```

### Layer 1 — Utility

Held from the left `ENT` thumb or the right `SPC` thumb.

```text
LEFT HALF                                                             RIGHT HALF
+----------+----------+----------+----------+----------+----------++----------------------+----------+----------+----------+----------+----------------------+----------+
| -        | -        | -        | TAB      | -        | -        || (                    | )        | UP       | [        | ]        | -                    | -        |
+----------+----------+----------+----------+----------+----------++----------------------+----------+----------+----------+----------+----------------------+----------+
| -        | -        | -        | -        | -        | -        || =                    | LEFT     | DOWN     | RIGHT    | -        | -                    | -        |
+----------+----------+----------+----------+----------+----------++----------------------+----------+----------+----------+----------+----------------------+----------+
| F2       | Ctrl+Up  | F3       | Prev     | Play     | Next     || Cmd+Ctrl+Shift+4     | -        | -        | -        | -        | Cmd+Shift+5          | Cmd+Shift+4 |
+----------+----------+----------+----------+----------+----------++----------------------+----------+----------+----------+----------+----------------------+----------+

THUMB STICK
          UP
LEFT - (none) - RIGHT
         DOWN
```

### Layer 2 — Number

Held from the left `ESC` thumb or the right `CAPS` thumb. The left half's top row
(`Q W E R T` positions) selects Bluetooth profile P1–P5.

```text
LEFT HALF                                                            RIGHT HALF
+----------+----------+----------+----------+----------+----------++----------+----------+----------+----------+----------+----------+
| USB      | P1       | P2       | P3       | P4       | P5       || 7        | 8        | 9        | -        | -        | -        |
+----------+----------+----------+----------+----------+----------++----------+----------+----------+----------+----------+----------+
| OBL      | -        | -        | -        | -        | -        || 4        | 5        | 6        | -        | -        | -        |
+----------+----------+----------+----------+----------+----------++----------+----------+----------+----------+----------+----------+
| BTC      | -        | -        | -        | -        | -        || 0        | 1        | 2        | 3        | -        | -        |
+----------+----------+----------+----------+----------+----------++----------+----------+----------+----------+----------+----------+

All three Bluetooth/output keys are on the **left half only**, at the position of
the Base-layer key shown in brackets:

| Key   | Base key | Binding          | What it does                                                              |
| ----- | -------- | ---------------- | ------------------------------------------------------------------------- |
| `USB` | `TAB`    | `&out OUT_USB`   | Latch output to USB. BLE stops advertising while wired.                   |
| `OBL` | `GRAVE`  | `&out OUT_BLE`   | Latch output to Bluetooth. Use after unplugging so output isn't stuck.    |
| `P1`–`P5` | `Q`–`T` | `&bt BT_SEL 0`–`4` | Select Bluetooth profile 1–5. Profile 1 is the default bond slot.    |
| `BTC` | `LSHFT`  | `&bt BT_CLR`     | Clear the bond of the **currently selected** profile (hold briefly).      |

> `BTC` acts on whichever profile is active — press `P1`–`P5` first, then `BTC`.
> A full board-wide re-pair still needs the `settings_reset` artifact.

THUMB STICK
          UP
LEFT - (none) - RIGHT
         DOWN
```

### Thumb scroller

On the **left half**. Rotating it sends volume keys on every layer.

| Direction         | Sends      |
| ----------------- | ---------- |
| Clockwise         | `C_VOL_UP` |
| Counter-clockwise | `C_VOL_DN` |

## Build & flash

Pushes trigger GitHub Actions; download artifacts from the **Actions** tab.

| Artifact                    | Flash to                                    |
| --------------------------- | ------------------------------------------- |
| `eyelash_corne_right`       | right half                                  |
| `eyelash_corne_studio_left` | left half (central, enables ZMK Studio)     |
| `settings_reset`            | either half, only to re-pair the two halves |

To flash: connect USB, double-tap **reset** within 0.5 s, then drag the
`.uf2` onto the mounted drive. Do not delete anything on the drive.

## Notes

- The shield, ZMK fork and required modules are pulled in via `config/west.yml`.
- Encoder direction is set in `config/eyelash_corne.keymap` (`rsr_vol`);
  swap `cw-binding` / `ccw-binding` if volume moves the wrong way.
- Hold `Q + S + Z` for 2 s to soft off.
- Bluetooth and output switching live on the Number layer, left half: `USB`
  (USB output), `OBL` (Bluetooth output), `P1`–`P5` (profile select), `BTC`
  (clear current profile's bond). See the Layer 2 legend.
- Settings (RGB, backlight, sleep, NKRO, debounce) live in `config/eyelash_corne.conf`.
