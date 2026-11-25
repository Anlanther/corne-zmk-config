# Eyelash Corne ZMK Config

ZMK firmware config for an **Eyelash Peripherals Corne** (`eyelash_corne` shield),
built on nice!nano v2 with a nice!view display on each half.

> This is **not** a foostan Corne. It is a 48-key, 5-row matrix and will not
> work with standard `corne` firmware.

## Hardware

| Part           | Detail                                                                               |
| -------------- | ------------------------------------------------------------------------------------ |
| Board          | `nice_nano_v2` ×2                                                                    |
| Shield         | `eyelash_corne_left` / `eyelash_corne_right` (from a741725193/zmk-new_corne)         |
| Display        | nice!view on both halves                                                             |
| Thumb scroller | EC11 encoder, **left half** — bound to volume (CCW down, CW up)                      |
| Thumb stick    | 5-way cross (`UP` `LEFT` `CLICK` `RIGHT` `DOWN`) — macOS shortcuts (see Thumb stick) |

## Layout

48 keys over 5 rows, split across two halves. The thumb cluster has 3 keys
per side; the stick sits between the halves in the upper-middle.

Legend: `key` = tap, `key / hold` where a key has two functions.
Blank cells are transparent placeholders (inherit from the layer below);
`-` is shown only where the key sends a minus,
`(none)` = no binding at all.

### Layer 0 — Base

```text
LEFT HALF                                            RIGHT HALF
+-------+-------+-------+-------+-------+-------++-------+-------+-------+-------+-------+-----------+
| TAB   | Q     | W     | E     | R     | T     || Y     | U     | I     | O     | P     | Caps Lock |
+-------+-------+-------+-------+-------+-------++-------+-------+-------+-------+-------+-----------+
| GRAVE | A     | S     | D     | F     | G     || H     | J     | K     | L     | SEMI  | SQT       |
|       | Shift | Ctrl  | Alt   | Cmd   |       ||       | Cmd   | Alt   | Ctrl  | Shift |           |
+-------+-------+-------+-------+-------+-------++-------+-------+-------+-------+-------+-----------+
| Shift | Z     | X     | C     | V     | B     || N     | M     | ,     | .     | /     | SHIFT     |
+-------+-------+-------+-------+-------+-------++-------+-------+-------+-------+-------+-----------+

THUMB KEYS
Left:  ESC (hold: Number) | ENT (hold: Utility) | DEL (hold: Cmd)
Right: BKSP               | SPC (hold: Utility) | \ (tap) / Number (hold)

THUMB STICK
          Mission Control
Launchpad - Spotlight - Opt+Esc
          Show Desktop
```

### Layer 1 — Utility

Held from the left `ENT` thumb or the right `SPC` thumb.

```text
LEFT HALF                                                             RIGHT HALF
+----------+----------+----------+----------+----------+----------++----------------------+----------+----------+----------+----------+----------------------+-------------+
|          |          |          | TAB      |          |          || (                    | )        | UP       | [        | ]        |                      | -           |
+----------+----------+----------+----------+----------+----------++----------------------+----------+----------+----------+----------+----------------------+-------------+
|          |          |          |          |          |          || =                    | LEFT     | DOWN     | RIGHT    |          |                      |             |
+----------+----------+----------+----------+----------+----------++----------------------+----------+----------+----------+----------+----------------------+-------------+
| F2       | Ctrl+Up  |          | Prev     | Play     | Next     || Cmd+Ctrl+Shift+4     |          |          |          |          | Cmd+Shift+5          | Cmd+Shift+4 |
+----------+----------+----------+----------+----------+----------++----------------------+----------+----------+----------+----------+----------------------+-------------+

THUMB STICK
Transparent — inherits the Base shortcuts (see "Thumb stick" below)
```

### Layer 2 — Number

Held from the left `ESC` thumb, or by holding the rightmost right thumb (`\`). The left
half's top row (at the `TAB` and `Q`–`T` positions) switches output and Bluetooth
profile.

```text
LEFT HALF                                                            RIGHT HALF
+----------+----------+----------+----------+----------+----------++----------+----------+----------+----------+----------+----------+
| USB      | P1       | P2       | P3       | P4       | P5       ||          | 7        | 8        | 9        |          |          |
+----------+----------+----------+----------+----------+----------++----------+----------+----------+----------+----------+----------+
| OBL      |          |          |          |          |          ||          | 4        | 5        | 6        |          |          |
+----------+----------+----------+----------+----------+----------++----------+----------+----------+----------+----------+----------+
| BTC      |          |          |          |          |          || 0        | 1        | 2        | 3        |          |          |
+----------+----------+----------+----------+----------+----------++----------+----------+----------+----------+----------+----------+

THUMB STICK
Transparent — inherits the Base shortcuts (see "Thumb stick" below)
```

The output and Bluetooth keys are on the **left half only**, replacing the
Base-layer key shown in brackets:

| Key       | Base key | Binding            | What it does                                                           |
| --------- | -------- | ------------------ | ---------------------------------------------------------------------- |
| `USB`     | `TAB`    | `&out OUT_USB`     | Latch output to USB. BLE stops advertising while wired.                |
| `OBL`     | `GRAVE`  | `&out OUT_BLE`     | Latch output to Bluetooth. Use after unplugging so output isn't stuck. |
| `P1`–`P5` | `Q`–`T`  | `&bt BT_SEL 0`–`4` | Select Bluetooth profile 1–5. Profile 1 is the default bond slot.      |
| `BTC`     | `LSHFT`  | `&bt BT_CLR`       | Clear the bond of the **currently selected** profile (hold briefly).   |

> `BTC` acts on whichever profile is active — press `P1`–`P5` first, then `BTC`.
> A full board-wide re-pair still needs the `settings_reset` artifact.

### Thumb stick

The stick's four directions plus its centre click are plain keys. They are bound
on Base and left transparent on Utility and Number, so the same macOS shortcuts
stay available there.

> `&trans` passes the key to the layer below — it does not disable it. If you want
> the stick inert on Utility/Number instead, replace those five cells with `&none`.

| Gesture | Base            | Binding                         |
| ------- | --------------- | ------------------------------- |
| Up      | Mission Control | `&kp MISSION_CONTROL` (`0x29F`) |
| Down    | Show Desktop    | `&kp F11`                       |
| Left    | Launchpad       | `&kp LAUNCHPAD` (`0x2A2`)       |
| Right   | Opt+Esc         | `&kp LA(ESC)`                   |
| Click   | Spotlight       | `&kp LG(SPACE)` (Cmd+Space)     |

Keymap positions for the cross are up `6`, left `19`, click `20`, right `21`,
down `35` (matrix column 12, rows 0–4).

> Down sends `F11`, which is macOS's default _Show Desktop_ shortcut
> (_System Settings → Keyboard → Keyboard Shortcuts → Mission Control_). If it does
> nothing, that shortcut has been reassigned.

### Thumb scroller

On the **left half**. Rotating it sends volume keys on every layer.

| Direction         | Sends      |
| ----------------- | ---------- |
| Clockwise         | `C_VOL_UP` |
| Counter-clockwise | `C_VOL_DN` |

## Build & flash

Pushes trigger GitHub Actions; download artifacts from the **Actions** tab.

| Firmware                        | Flash to                                    |
| ------------------------------- | ------------------------------------------- |
| `eyelash_corne_right.uf2`       | right half                                  |
| `eyelash_corne_studio_left.uf2` | left half (central, enables ZMK Studio)     |
| `settings_reset.uf2`            | either half, only to re-pair the two halves |

To flash: connect USB, double-tap **reset** within 0.5 s, then drag the
`.uf2` onto the mounted drive. Do not delete anything on the drive.

## Notes

- Encoder direction is set in `config/eyelash_corne.keymap` (`rsr_vol`);
  swap `cw-binding` / `ccw-binding` if volume moves the wrong way.
- Hold `Q + S + Z` for 2 s to soft off.
- Hold-taps (`hm`, `thumb_lt`, `thumb_lt_own`) are `balanced` with
  `hold-trigger-on-release`, `tapping-term-ms = 200` and
  `require-prior-idle-ms = 175`: a hold only wins if the interrupting key is still
  down when the hold key is released, and anything typed within 175 ms of the
  previous key is always a tap, so fast rolls no longer become stuck modifiers.
  These timings are tuned to the owner's typing profile (90–100 WPM, fast rolls,
  deliberate home-row chords) — see `docs/user-preference/typing-profile.md` (if
  present) before changing them.
- **Caps Lock must be a plain key on macOS.** It is bound as a plain `&kp CAPS`
  (currently the top-right key), not as the tap action of a hold-tap. macOS ignores a
  Caps Lock press shorter than ~80 ms (QMK's `TAP_HOLD_CAPS_DELAY`), and ZMK emits
  a hold-tap's tap action with ~0 ms press duration, so putting `CAPS` as the tap
  of `&lt`/`&thumb_lt` silently does nothing (ZMK issue
  [#756](https://github.com/zmkfirmware/zmk/issues/756)). If you need Caps Lock on
  a layer-tap, make the tap a macro with `tap-ms` (~100 ms) instead.
- Settings (RGB, backlight, sleep, NKRO, debounce) live in `config/eyelash_corne.conf`.
