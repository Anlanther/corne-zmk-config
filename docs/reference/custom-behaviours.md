# Custom Behaviours

The Corne config goes beyond plain ZMK by defining a few extra behaviours. All are declared in `config/corne.keymap` above the `keymap { … }` node.

## `&td0` tap-dance

Top-left alpha key. Defined as:

```dts
/ {
    behaviors {
        td0: td0 {
            compatible = "zmk,behavior-tap-dance";
            label = "TD0";
            #binding-cells = <0>;
            bindings = <&kp LEFT_SHIFT>, <&kp CAPS>;
        };
    };
};
```

Behaviour on the default layer (layer 0):

| Gesture    | Output     |
| ---------- | ---------- |
| tap        | `LSHIFT`   |
| hold       | `LSHIFT`   |
| double-tap | `CAPSLOCK` |

How it works: `tap-dance` sequences are the ordered list in `bindings`. Tap-dance with a hold-capable first entry passes through the first binding on each tap. To add more taps, add more `&kp …` entries to `bindings`. To make a _different_ key do this, copy the `td0` node as `td1` and reference `&td1` in the keymap — `#binding-cells` stays `<0>` because the key takes no parameter.

> Tap-dance adds a small timing delay to single taps (it waits for the tap window). If a key feels laggy, that's why — prefer `&mt`/`&lt` for shifted keys unless you actually need multi-tap.

## Layer-tap thumb keys

```dts
&lt 3 SPACE     // left thumb: tap = Space, hold = layer 3
&lt 3 ENTER     // right thumb: tap = Enter, hold = layer 3
```

`&lt <layer> <key>` is ZMK's `hold-tap` layer behaviour. The firmware configures a "balanced" flavour; the relevant knobs (in `corne.dtsi`/`corne_left.dts` under a `behaviors` node with `compatible = "zmk,behavior-hold-tap"`) are:

| Property                | Effect                                                                                                     |
| ----------------------- | ---------------------------------------------------------------------------------------------------------- |
| `tapping-term-ms`       | how long a hold must last to count as a hold                                                               |
| `quick-tap-ms`          | re-tap within this window always sends the tap key                                                         |
| `require-prior-idle-ms` | only treat as hold if the keyboard was idle this long (reduces accidental layer changes while typing fast) |
| `flavor`                | `balanced`, `tap-preferred`, `hold-preferred`, `tap-unless-interrupted`                                    |

If you experience "space became layer 3 mid-word", lengthen `require-prior-idle-ms` or switch to `tap-preferred`.

## Rotary encoder

The encoder is a _sensor_; its action per layer is set by the layer's `sensor-bindings`:

```dts
sensor-bindings = <&inc_dec_kp C_VOL_UP C_VOL_DN>;   // layer 0: volume
```

Typical layer-specific variants used by the shipped keymap:

| Layer | Sensor binding                  | Effect             |
| ----- | ------------------------------- | ------------------ |
| 0     | `&inc_dec_kp C_VOL_UP C_VOL_DN` | volume             |
| 1–3   | `&msc SCRL_UP SCRL_DOWN`        | mouse-wheel scroll |

Encoder _press_ is a normal matrix key (left thumb area), currently mapped to `SPACE` (`&kp SPACE`) on layer 0.

When re-keying the encoder in DYA STUDIO, the delay field must be ≥ 50 ms or the change is rejected (see [End-user behaviour reference → DYA STUDIO](user-reference.md#dya-studio-real-time-re-keying)).

## Mouse / joystick layer

The center cluster is a joystick sensor; its bindings emulate mouse movement:

```dts
&mmv MOVE_UP / MOVE_DOWN / MOVE_LEFT / MOVE_RIGHT   // cursor move
&mkp LCLK | RCLK | MCLK | MB4 | MB5                // clicks
```

On layer 0 the same area maps to arrow keys and `&kp ENTER`, so the "joystick" is only a cursor on the upper layers. If you want acceleration/tuning, ZMK exposes `CONFIG_ZMK_POINTING_…` options (see [config options](config-options.md)).

## RGB power saving

`&ext_power EP_ON` / `&ext_power EP_OFF` were the original way to cut LED power. **Newer firmware removed them and ties the LED supply to the underglow state**: `RGB_OFF` cuts the positive rail, `RGB_ON` restores it. If you are on the newer firmware, do not re-add `EP_*` — it's redundant and can only cause "battery drains" confusion.

Per-LED current is ~15 mA, so a 30-key half with all LEDs on draws ~450 mA — avoid leaving the underglow on for battery builds.

## Adding your own behaviour

Example — a "hold Shift for 200 ms after a tap" style combo using ZMK's `mod-morph`:

```dts
/ {
    behaviors {
        esc_rshift: mm_esc_rshift {
            compatible = "zmk,behavior-mod-morph";
            #binding-cells = <0>;
            bindings = <&kp ESC>, <&kp RSHIFT>;
            mods = <(MOD_LSFT|MOD_RSFT)>;
        };
    };
};
```

Then use `&esc_rshift` in a keymap. Rules:

- `#binding-cells` must equal the number of parameters the keycode supplies (0 for a no-arg behaviour).
- Put all behaviour nodes before `keymap { … }`.
- Reference upstream behaviour docs: <https://zmk.dev/docs/keymaps/behaviors>.
