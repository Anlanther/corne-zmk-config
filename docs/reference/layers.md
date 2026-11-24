# Layer & Keymap Reference

The Corne ships with **4 layers**. Names come from the shipped layer diagrams; a text transcription of every layer is in [Keymap diagrams](keymap-diagrams.md).

| Index | DTS node name   | Display name | Entered by                  |
| ----- | --------------- | ------------ | --------------------------- |
| 0     | `default_layer` | `QWERTY`     | always active               |
| 1     | `lower_layer`   | `NUMBER`     | hold left thumb `&mo 1`     |
| 2     | `raise_layer`   | `SYMBOL`     | hold right thumb `&mo 2`    |
| 3     | `layer_3`       | `FN`         | hold either thumb `&lt 3 …` |

## Skeleton

```dts
/ {
    keymap {
        compatible = "zmk,keymap";

        default_layer {           // layer 0 — QWERTY
            display-name = "QWERTY";
            bindings = <
                &kp TAB   &kp Q    &kp W  &kp E  &kp R  &kp T     &kp Y  &kp U  &kp I    &kp O   &kp P    &kp BSPC
                &td0      &kp A    &kp S  &kp D  &kp F  &kp G     &kp H  &kp J  &kp K    &kp L   &kp SEMI &kp SQT
                &kp GRAVE &kp Z    &kp X  &kp C  &kp V  &kp B     &kp N  &kp M  &kp COMMA &kp DOT &kp SLASH &kp ESC
                                  &kp LGUI &mo 1 &lt 3 SPACE     &lt 3 ENTER &mo 2 &kp RALT
            >;
        };

        lower_layer {             // layer 1 — NUMBER
            display-name = "NUMBER";
            bindings = < … >;
        };

        raise_layer {             // layer 2 — SYMBOL
            display-name = "SYMBOL";
            bindings = < … >;
        };

        layer_3 {                 // layer 3 — FN
            display-name = "FN";
            bindings = < … >;
        };
    };
};
```

`display-name` is what the screen shows at the bottom (`LAYER0` in the photos is the default when no name is set). Keep `display-name` in sync if you rename a layer.

## Layer 0 — QWERTY

- Thumb cluster: `&kp LGUI` · `&mo 1` · `&lt 3 SPACE` | `&lt 3 ENTER` · `&mo 2` · `&kp RALT`
- Top-left alpha key is `&td0` (tap/hold = Shift, double-tap = Caps Lock — see [custom behaviours](custom-behaviours.md#td0-tap-dance)).
- The middle "arrow keys" in the picture are the **right-hand joystick's default sensor bindings**, not matrix keys.

## Layer 1 — NUMBER

| Position         | Binding                                                                                        |
| ---------------- | ---------------------------------------------------------------------------------------------- |
| top row          | `&kp N1` … `&kp N0`                                                                            |
| left hand        | `&kp INS`-adjacent cluster: `BT_CLR_ALL`, `BT_SEL 0`…`BT_SEL 3`                                |
| left hand bottom | `RGB_OFF`, `RGB_ON`, `&ext_power EP_OFF`                                                       |
| right hand       | `RGB_EFF`, `RGB_EFR`, `RGB_BRI`, `RGB_BRD`, `RGB_SPI`, `HOME`, `END`, `PG_UP`, `PG_DN`, arrows |
| center           | mouse move (`&mmv MOVE_*`) + `&mkp LCLK`                                                       |

Symbol-level names taken from the diagram: `&bt BT_CLR_ALL`, `&bt BT_SEL 0`, `&rgb_ug RGB_OFF|RGB_ON|RGB_EFF`, `&ext_power EP_OFF`, `&rgb_ug RGB_EFR|RGB_BRI|RGB_BRD|RGB_SPI`, `&kp HOME|END|PG_UP|PG_DN|INS`.

> Newer firmware removed `EP_ON` and ties RGB power to the underglow state, so `EP_OFF` may be absent from your keymap. Harmless if present.

## Layer 2 — SYMBOL

Hold `&mo 2` and press the key printed with the symbol:

```text
!  @  #  $  %     ^  &  *  (  )
-  =  [  ]  \     `  +  {  }  |
~  '  OUT_USB/OUT_BLE
```

- `&out OUT_USB` / `&out OUT_BLE` force the report channel (USB wire vs Bluetooth).
- `&none` entries are intentionally dead — keep them if you like the symmetry, or replace them.

## Layer 3 — FN

- `&kp F1` … `&kp F12`, `&kp PSCRN`, `&kp SLCK`, `&kp PAUSE_BREAK`
- `&sys_reset` (reboots and refreshes the screen), `&bootloader` (enters UF2 mode), `&soft_off` (deep sleep)
- Mouse: `&mkp LCLK|MCLK|RCLK|MB4|MB5`, plus `&mmv MOVE_*` on the center cluster

Common access pattern: hold Enter (`&lt 3 ENTER`) and press `Q` to send `F1`.

## Editing cheat-sheet

| I want to…                    | Do this                                      |
| ----------------------------- | -------------------------------------------- |
| swap two keys                 | swap the two `&kp …` tokens                  |
| disable a key                 | replace with `&none`                         |
| keep a key on the layer below | replace with `&trans`                        |
| move a key later              | comment it out with `//` instead of deleting |
| add a hold-layer              | `&mo <index>`                                |
| add hold-mod / tap-key        | `&mt <MOD> <KEY>` (e.g. `&mt LSHIFT A`)      |
| add hold-layer / tap-key      | `&lt <index> <KEY>`                          |

Look up any keycode name at <https://zmk.dev/docs/codes>. The names in the diagrams (`&kp EXCL`, `&kp AT`, `&kp HASH`) are the same names used in `.keymap`.

## Sofle differences

Sofle has 3 layers in the shipped diagrams (`layer0`, `layer1`, `layer2`), enters layer 1 from the **left** thumb and layer 2 from the **right** thumb, and has an extra `C_MUTE` key near the encoder. It has no `&td0` in the diagram; treat the Corne tables above as the reference implementation.
