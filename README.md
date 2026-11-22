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

```mermaid
flowchart TB
    subgraph L["Left half"]
        l1["TAB"] --- l2["Q"] --- l3["W"] --- l4["E"] --- l5["R"] --- l6["T"]
        l7["GRAVE"] --- l8["A<br/>Shift"] --- l9["S<br/>Ctrl"] --- l10["D<br/>Alt"] --- l11["F<br/>Cmd"] --- l12["G"]
        l13["Shift"] --- l14["Z"] --- l15["X"] --- l16["C"] --- l17["V"] --- l18["B"]
    end
    subgraph S["Thumb stick"]
        s1["UP"]
        s2["LEFT"] --- s3["(none)"] --- s4["RIGHT"]
        s5["DOWN"]
    end
    subgraph R["Right half"]
        r1["Y"] --- r2["U"] --- r3["I"] --- r4["O"] --- r5["P"] --- r6["\\"]
        r7["H"] --- r8["J<br/>Cmd"] --- r9["K<br/>Alt"] --- r10["L<br/>Ctrl"] --- r11["SEMI<br/>Shift"] --- r12["SQT"]
        r13["N"] --- r14["M"] --- r15[","] --- r16["."] --- r17["/"] --- r18["ESC"]
    end
    subgraph T["Thumb keys"]
        t1["ESC<br/>hold: Number"] --- t2["ENT<br/>hold: Utility"] --- t3["DEL<br/>hold: Cmd"]
        t4["BKSP"] --- t5["SPC<br/>hold: Utility"] --- t6["CAPS<br/>hold: Number"]
    end
```

### Layer 1 — Utility

Held from the left `ENT` thumb or the right `SPC` thumb.

```mermaid
flowchart TB
    subgraph L["Left half"]
        l1["-"] --- l2["-"] --- l3["-"] --- l4["TAB"] --- l5["-"] --- l6["-"]
        l7["-"] --- l8["-"] --- l9["-"] --- l10["-"] --- l11["-"] --- l12["-"]
        l13["F2"] --- l14["Ctrl+Up"] --- l15["F3"] --- l16["Prev"] --- l17["Play"] --- l18["Next"]
    end
    subgraph S["Thumb stick"]
        s1["UP"]
        s2["LEFT"] --- s3["(none)"] --- s4["RIGHT"]
        s5["DOWN"]
    end
    subgraph R["Right half"]
        r1["("] --- r2[")"] --- r3["↑"] --- r4["["] --- r5["]"] --- r6["-"]
        r7["="] --- r8["←"] --- r9["↓"] --- r10["→"] --- r11["-"] --- r12["-"]
        r13["Cmd+Ctrl+Shift+4"] --- r14["-"] --- r15["-"] --- r16["-"] --- r17["Cmd+Shift+5"] --- r18["Cmd+Shift+4"]
    end
```

### Layer 2 — Number

Held from the left `ESC` thumb or the right `CAPS` thumb.

```mermaid
flowchart TB
    subgraph L["Left half"]
        l1["-"] --- l2["-"] --- l3["-"] --- l4["-"] --- l5["-"] --- l6["-"]
        l7["-"] --- l8["-"] --- l9["-"] --- l10["-"] --- l11["-"] --- l12["-"]
        l13["-"] --- l14["-"] --- l15["-"] --- l16["-"] --- l17["-"] --- l18["-"]
    end
    subgraph S["Thumb stick"]
        s1["UP"]
        s2["LEFT"] --- s3["(none)"] --- s4["RIGHT"]
        s5["DOWN"]
    end
    subgraph R["Right half"]
        r1["7"] --- r2["8"] --- r3["9"] --- r4["-"] --- r5["-"] --- r6["-"]
        r7["4"] --- r8["5"] --- r9["6"] --- r10["-"] --- r11["-"] --- r12["-"]
        r13["0"] --- r14["1"] --- r15["2"] --- r16["3"] --- r17["-"] --- r18["-"]
    end
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
- Settings (RGB, backlight, sleep, NKRO, debounce) live in `config/eyelash_corne.conf`.
