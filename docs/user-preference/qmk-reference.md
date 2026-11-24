# ZSA Voyager — QMK reference

> **Record only — do not follow these key bindings.** This file documents a
> different keyboard (a ZSA Voyager running QMK/Oryx) and is kept only to record
> the owner's Voyager layout. The authoritative keymap for this repo is
> `config/eyelash_corne.keymap`; never copy bindings, layers, or keycodes from
> this file into the Corne ZMK config. If the owner asks you to approximate
> something from it, translate the intent deliberately rather than mirroring it.

QMK translation of the ZSA Oryx layout
<https://configure.zsa.io/voyager/layouts/EmXGb/latest/0>.
Data was read from the Oryx GraphQL API (`https://oryx.zsa.io/graphql`,
revision `AzXBeZ`) on 2026-11-24.
This is a documentary reference for the Voyager layout on the ZSA Oryx
platform; it is unrelated to the Eyelash Corne ZMK config in `config/`.

## Metadata

| Field                | Value                                              |
| -------------------- | -------------------------------------------------- |
| Keyboard             | ZSA Voyager (`voyager` geometry)                   |
| Layout id            | `EmXGb`                                            |
| Layout title         | Default                                            |
| Revision             | `AzXBeZ`                                           |
| QMK / Oryx           | QMK 25.0, up to date                               |
| Firmware md5         | `ee3765b6b46d6b967bf4260e3f643dfa`                 |
| Model                | `v1` (no alternate MCU)                            |
| Layers               | 4 — 0 `Base`, 1 `Second`, 2 `Neutral`, 3 `Colours` |
| Combos               | none                                               |
| Macros               | none                                               |
| Navigators / sensors | none                                               |
| Deleted layers       | no                                                 |

## Global configuration

The Oryx revision-level `config` object is the only global configuration
besides the layer contents:

```json
{
  "autoshiftTimeout": 50,
  "disabledAnimations": []
}
```

- `autoshiftTimeout: 50` — Oryx stores a 50 ms auto-shift timeout. No
  auto-shift key is used in any layer, so this has no visible effect.
- `disabledAnimations: []` — every RGB animation is enabled.

No `RGB_*`, tapping-term, hold-tap, or other `rules.mk`/`config.h` overrides
are stored in the layout; shipping Voyager defaults apply.

## How Oryx keys map to QMK

| Oryx key data                           | QMK equivalent                                                         |
| --------------------------------------- | ---------------------------------------------------------------------- |
| `tap` only                              | `KC_*`                                                                 |
| `tap` + `hold` modifier                 | `MT(MOD_Lxxx, KC_TAP)` (e.g. `LSFT_T(KC_A)`)                           |
| `tap` + `hold` `MO` layer _n_           | `LT(n, KC_TAP)`                                                        |
| `tap.modifiers`                         | nested mod macros, e.g. `LSFT(LGUI(KC_5))`                             |
| `doubleTap`                             | QMK **Tap Dance**, `ACTION_TAP_DANCE_DOUBLE(single, double)`           |
| blank key (no `tap`/`hold`/`doubleTap`) | `KC_TRANSPARENT`                                                       |
| explicit “Transparent”                  | `KC_TRANSPARENT`                                                       |
| explicit “None”                         | `KC_NO` (not used here)                                                |
| `RGB` with a colour                     | Oryx/Voyager fixed-colour underglow key (colour stored in `tap.color`) |

Index convention: **Oryx key index _i_ is QMK key `kNN` with `NN = i`**.
The `LAYOUT` argument order differs (QMK interleaves the halves row by row);
the appendix lists the bindings in the real `LAYOUT_voyager(...)` order.

Diagram legend: first line = tap action, second line = hold / double-tap.
`—` = transparent (inherit from lower layer), `L1`/`L2`/`L3` = layer hold,
`2x …` = on double tap, `⌃` Ctrl, `⇧` Shift, `⌥` Alt, `⌘` Cmd.

## Layer 0 — Base

Default layer.

```text
                      LEFT HAND                        ||                       RIGHT HAND
 `        1        2        3        4        5        ||  6        7        8        9        0        -
                                                       ||
 TAB      Q        W        E        R        T        ||  Y        U        I        O        P        \
                                                       ||
 CAPS     A        S        D        F        G        ||  H        J        K        L        ;        '
          ⇧        ⌃        ⌥        ⌘                 ||           ⌘        ⌥        ⌃        ⇧
 Shift    Z        X        C        V        B        ||  N        M        ,        .        /        Shift
 2x ESC                                                ||                                               2x ⌥ESC
THUMBS  left:  ENT/L1 (k24)   DEL/⌘ (k25)
        right: BKSP (k50)   SPC/L1 (k51)
```

Notable bindings on this layer:

| Key   | Binding                        |
| ----- | ------------------------------ |
| `k13` | tap `A`, hold `⇧`              |
| `k14` | tap `S`, hold `⌃`              |
| `k15` | tap `D`, hold `⌥`              |
| `k16` | tap `F`, hold `⌘`              |
| `k18` | tap `Shift`, double-tap `ESC`  |
| `k24` | tap `ENT`, hold layer 1        |
| `k25` | tap `DEL`, hold `⌘`            |
| `k39` | tap `J`, hold `⌘`              |
| `k40` | tap `K`, hold `⌥`              |
| `k41` | tap `L`, hold `⌃`              |
| `k42` | tap `;`, hold `⇧`              |
| `k49` | tap `Shift`, double-tap `⌥ESC` |
| `k51` | tap `SPC`, hold layer 1        |

## Layer 1 — Second

```text
                         LEFT HAND                           ||                          RIGHT HAND
 KC_F2     Mission   KC_F3     PREV      PLAY      NEXT      ||  Cap Copy  MUTE      VOL-      VOL+      Prev Tab  Next Tab
                                                             ||
 —         Sleep     ESC       TAB       —         —         ||  —         —         UP        [         ]         TG2
           2x ⌘⌃Q                                            ||
 —         —         —         —         —         —         ||  =         LEFT      DOWN      RIGHT     —         TG3
                                                             ||
 —         —         —         —         —         —         ||  —         —         —         —         Video     Cap
                                                             ||
THUMBS  left:  — (k24)   — (k25)
        right: — (k50)   — (k51)
```

Notable bindings on this layer:

| Key   | Binding                         |
| ----- | ------------------------------- |
| `k07` | double-tap `⌘⌃Q`, label “Sleep” |
| `k26` | tap `⌘⌃⇧4`, label “Cap Copy”    |
| `k30` | tap `⌘⇧[`, label “Prev Tab”     |
| `k31` | tap `⌘⇧]`, label “Next Tab”     |
| `k37` | tap `TG2`                       |
| `k43` | tap `TG3`                       |
| `k48` | tap `⌘⇧5`, label “Video”        |
| `k49` | tap `⌘⇧4`, label “Cap”          |

## Layer 2 — Neutral

```text
                   LEFT HAND                     ||                    RIGHT HAND
 —       —       —       —       —       —       ||  —       —       —       —       —       —
                                                 ||
 —       —       —       —       —       —       ||  —       —       —       —       —       —
                                                 ||
 —       A       S       D       F       —       ||  —       J       K       L       ;       —
                                                 ||
 —       —       —       —       —       —       ||  —       —       —       —       —       —
                                                 ||
THUMBS  left:  — (k24)   — (k25)
        right: — (k50)   — (k51)
```

## Layer 3 — Colours

```text
                         LEFT HAND                           ||                          RIGHT HAND
 —         —         —         —         —         —         ||  RGB       RGB       —         —         —         —
                                                             ||  #42FF00   #FF0000
 —         —         —         —         —         —         ||  RGB       RGBTOG    LAYERCOL  RGBMOD    —         —
                                                             ||  #00F0FF
 —         —         —         —         —         —         ||  RGB       RGBSLD    BRI-      BRI+      —         —
                                                             ||  #0075FF
 —         —         —         —         —         —         ||  RGB       HUE-      HUE+      SAT-      SAT+      —
                                                             ||  #000EFF
THUMBS  left:  — (k24)   — (k25)
        right: — (k50)   — (k51)
```

Notable bindings on this layer:

| Key   | Binding                     |
| ----- | --------------------------- |
| `k26` | tap `RGB`, colour `#42FF00` |
| `k27` | tap `RGB`, colour `#FF0000` |
| `k32` | tap `RGB`, colour `#00F0FF` |
| `k38` | tap `RGB`, colour `#0075FF` |
| `k44` | tap `RGB`, colour `#000EFF` |

## Behaviour summary

### Home-row mods

The four home keys of each hand are mod-taps (tap a letter, hold a
modifier). They are symmetric and _not_ the usual “same mod on both hands”
arrangement — the right hand uses the right-side modifiers:

| Position  | Left hand      | Right hand        |
| --------- | -------------- | ----------------- |
| inner     | `LSFT_T(KC_A)` | `RSFT_T(KC_SCLN)` |
| mid-inner | `LCTL_T(KC_S)` | `RCTL_T(KC_L)`    |
| mid-outer | `LALT_T(KC_D)` | `RALT_T(KC_K)`    |
| outer     | `LGUI_T(KC_F)` | `RGUI_T(KC_J)`    |

### Thumb layer-taps

| Key                     | Tap         | Hold                     |
| ----------------------- | ----------- | ------------------------ |
| left inner thumb `k24`  | `KC_ENTER`  | layer 1 (`Second`)       |
| right inner thumb `k51` | `KC_SPACE`  | layer 1 (`Second`)       |
| left outer thumb `k25`  | `KC_DELETE` | `KC_LEFT_GUI` (`LGUI_T`) |
| right outer thumb `k50` | `KC_BSPC`   | — (plain key)            |

### Double-tap (Tap Dance) keys

| Key   | Layer  | Single tap       | Double tap                            |
| ----- | ------ | ---------------- | ------------------------------------- |
| `k18` | Base   | `KC_LEFT_SHIFT`  | `KC_ESCAPE`                           |
| `k49` | Base   | `KC_RIGHT_SHIFT` | `RALT(KC_ESCAPE)` (Opt+Esc)           |
| `k07` | Second | _(nothing)_      | `LCTL(LGUI(KC_Q))` (macOS lock/sleep) |

### Custom-labelled keys (macOS shortcuts)

These keys carry an Oryx `customLabel`; the tap code/modifier is the actual
keycode that is sent.

| Key   | Layer  | Label    | Sends                                       |
| ----- | ------ | -------- | ------------------------------------------- |
| `k07` | Second | Sleep    | double-tap `LCTL(LGUI(KC_Q))`               |
| `k26` | Second | Cap Copy | `LCTL(LSFT(LGUI(KC_4)))` (Cmd+Ctrl+Shift+4) |
| `k30` | Second | Prev Tab | `LSFT(LGUI(KC_LBRC))` (Cmd+Shift+[)         |
| `k31` | Second | Next Tab | `LSFT(LGUI(KC_RBRC))` (Cmd+Shift+])         |
| `k49` | Second | Cap      | `LSFT(LGUI(KC_4))` (Cmd+Shift+4)            |
| `k48` | Second | Video    | `LSFT(LGUI(KC_5))` (Cmd+Shift+5)            |

### RGB / colour keys (Colours layer)

The Voyager uses its RGB matrix. The `RGB` keys with a colour are Oryx
“RGB colour” keys; the hex value is in the key data. `RGB_SLD` and
`TOGGLE_LAYER_COLOR` are ZSA/Voyager-specific keycodes (see
`keyboards/zsa/voyager`); `RGB_MODE_FORWARD` is Oryx's name for QMK
`RGB_MOD`. `RGB_TOG`, `RGB_VAI/VAD`, `RGB_HUI/HUD` and `RGB_SAI/SAD` are
standard QMK keycodes.

| Key   | Binding              | Colour  |
| ----- | -------------------- | ------- |
| `k26` | `RGB`                | #42FF00 |
| `k27` | `RGB`                | #FF0000 |
| `k32` | `RGB`                | #00F0FF |
| `k33` | `RGB_TOG`            | —       |
| `k34` | `TOGGLE_LAYER_COLOR` | —       |
| `k35` | `RGB_MODE_FORWARD`   | —       |
| `k38` | `RGB`                | #0075FF |
| `k39` | `RGB_SLD`            | —       |
| `k40` | `RGB_VAD`            | —       |
| `k41` | `RGB_VAI`            | —       |
| `k44` | `RGB`                | #000EFF |
| `k45` | `RGB_HUD`            | —       |
| `k46` | `RGB_HUI`            | —       |
| `k47` | `RGB_SAD`            | —       |
| `k48` | `RGB_SAI`            | —       |

## Appendix — equivalent QMK bindings in `LAYOUT_voyager` order

Arguments below follow the real QMK `LAYOUT` order (row-interleaved, see
`keyboards/zsa/voyager/keyboard.json`), using Oryx index → QMK `kNN`.
Oryx `MAC_MISSION_CONTROL` is written as `KC_MISSION_CONTROL` and
`RGB_MODE_FORWARD` as `RGB_MOD`. Keys marked `TD(...)` need the tap dances
declared first; `TOGGLE_LAYER_COLOR` and `RGB_SLD` are ZSA custom keycodes,
and the Oryx `RGB` colour keys have no stock-QMK keycode (the colour is
shown in a comment).

```c
enum layers { _BASE, _SECOND, _NEUTRAL, _COLOURS };

enum tap_dance { TD_ESC, TD_ESC_OPT, TD_SLEEP };
tap_dance_action_t tap_dance_actions[] = {
    [TD_ESC]     = ACTION_TAP_DANCE_DOUBLE(KC_LSFT, KC_ESC),
    [TD_ESC_OPT] = ACTION_TAP_DANCE_DOUBLE(KC_RSFT, RALT(KC_ESC)),
    [TD_SLEEP]   = ACTION_TAP_DANCE_DOUBLE(KC_NO, LCTL(LGUI(KC_Q))),
};

// Layer 0 — Base
LAYOUT_voyager(
    KC_GRAVE, KC_1, KC_2, KC_3, KC_4, KC_5,
    KC_6, KC_7, KC_8, KC_9, KC_0, KC_MINUS,
    KC_TAB, KC_Q, KC_W, KC_E, KC_R, KC_T,
    KC_Y, KC_U, KC_I, KC_O, KC_P, KC_BSLS,
    KC_CAPS, MT(MOD_LSFT, KC_A), MT(MOD_LCTL, KC_S), MT(MOD_LALT, KC_D), MT(MOD_LGUI, KC_F), KC_G,
    KC_H, MT(MOD_RGUI, KC_J), MT(MOD_RALT, KC_K), MT(MOD_RCTL, KC_L), MT(MOD_RSFT, KC_SCLN), KC_QUOTE,
    TD(TD_ESC), KC_Z, KC_X, KC_C, KC_V, KC_B,
    KC_N, KC_M, KC_COMMA, KC_DOT, KC_SLASH, TD(TD_ESC_OPT),
    LT(1, KC_ENTER), MT(MOD_LGUI, KC_DELETE), KC_BSPC, LT(1, KC_SPACE)
),

// Layer 1 — Second
LAYOUT_voyager(
    KC_F2, KC_MISSION_CONTROL, KC_F3, KC_MEDIA_PREV_TRACK, KC_MEDIA_PLAY_PAUSE, KC_MEDIA_NEXT_TRACK,
    LCTL(LSFT(LGUI(KC_4))), KC_AUDIO_MUTE, KC_AUDIO_VOL_DOWN, KC_AUDIO_VOL_UP, LSFT(LGUI(KC_LBRC)), LSFT(LGUI(KC_RBRC)),
    KC_TRANSPARENT, TD(TD_SLEEP), KC_ESCAPE, KC_TAB, KC_TRANSPARENT, KC_TRANSPARENT,
    KC_TRANSPARENT, KC_TRANSPARENT, KC_UP, KC_LBRC, KC_RBRC, TG(2),
    KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT,
    KC_EQUAL, KC_LEFT, KC_DOWN, KC_RIGHT, KC_TRANSPARENT, TG(3),
    KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT,
    KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, LSFT(LGUI(KC_5)), LSFT(LGUI(KC_4)),
    KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT
),

// Layer 2 — Neutral
LAYOUT_voyager(
    KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT,
    KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT,
    KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT,
    KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT,
    KC_TRANSPARENT, KC_A, KC_S, KC_D, KC_F, KC_TRANSPARENT,
    KC_TRANSPARENT, KC_J, KC_K, KC_L, KC_SCLN, KC_TRANSPARENT,
    KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT,
    KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT,
    KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT
),

// Layer 3 — Colours
LAYOUT_voyager(
    KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT,
    KC_NO /* RGB #42FF00 */, KC_NO /* RGB #FF0000 */, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT,
    KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT,
    KC_NO /* RGB #00F0FF */, RGB_TOG, TOGGLE_LAYER_COLOR, RGB_MOD, KC_TRANSPARENT, KC_TRANSPARENT,
    KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT,
    KC_NO /* RGB #0075FF */, RGB_SLD, RGB_VAD, RGB_VAI, KC_TRANSPARENT, KC_TRANSPARENT,
    KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT,
    KC_NO /* RGB #000EFF */, RGB_HUD, RGB_HUI, RGB_SAD, RGB_SAI, KC_TRANSPARENT,
    KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT, KC_TRANSPARENT
),
```
