# Keymap Diagrams (transcribed)

Text version of the shipped layer pictures. The originals were screenshots from the keymap editor; this transcription is what you should rely on if the images aren't available (and works standalone when this folder is copied elsewhere).

Corne: 4 layers. Sofle: 3 layers (covered at the end).

## How to read the notation

- Each key shows its binding: `&kp W`, `&mo 1`, `&lt 3 SPACE`, `&bt BT_SEL 1`, `&rgb_ug RGB_ON`.
- `&trans` — transparent: behaves as on the layer below.
- `&none` — outputs nothing.
- The "Sensor bindings" block in the corner of each picture is the **rotary encoder**. On layer 0 it is volume; on the other layers it scrolls the wheel.

## Corne — layer 0 · `default_layer` · QWERTY

```text
L: TAB    Q    W    E    R    T            Y    U    I    O    P    BSPC
   td0    A    S    D    F    G            H    J    K    L    ;    '
   `      Z    X    C    V    B            N    M    ,    .    /    ESC
                   LGUI  mo1  lt3-SPC     lt3-ENT  mo2  RALT
```

- `td0` (top-left alpha) = tap/hold `LSHIFT`, double-tap `CAPS`.
- `mo1` hold → layer 1; `mo2` hold → layer 2; `lt3-SPC`/`lt3-ENT` hold → layer 3, tap → Space/Enter.
- The arrow cluster in the middle of the picture is the **joystick**, not matrix keys.
- Encoder: volume up / down.

## Corne — layer 1 · `lower_layer` · NUMBER

```text
L: trans  1    2    3    4    5            6    7    8    9    0    BSPC
   trans  BT_CLR_ALL  BT_SEL0  BT_SEL1  BT_SEL2  BT_SEL3       arrows / mouse move
   trans  RGB_OFF     RGB_ON   EP_OFF   RGB_EFF                RGB_EFR RGB_SPI RGB_BRI RGB_BRD
                   trans trans trans      INS  LCLK  HOME END PG_UP PG_DN
```

- Bluetooth: `&bt BT_CLR_ALL`, `&bt BT_SEL 0` … `&bt BT_SEL 3`.
- RGB: `&rgb_ug RGB_OFF|RGB_ON|RGB_EFR|RGB_EFF|RGB_SPI|RGB_BRI|RGB_BRD`; power cut via `&ext_power EP_OFF`.
- Navigation: `HOME`, `END`, `PG_UP`, `PG_DN`, `INS`, arrows.
- Center cluster: mouse movement `&mmv MOVE_*` with `&mkp LCLK`.
- Newer firmware: RGB off already cuts the LED rail, so `EP_OFF` is redundant/removed.

## Corne — layer 2 · `raise_layer` · SYMBOL

```text
L: trans  !    @    #    $    %               ^    &    *    (    )
   trans  LCLK MCLK RCLK MB4  MB5             `    =    [    ]    \
   trans  OUT_USB  OUT_BLE  none  none        +    {    }    |    ~
                   trans trans lt3             lt3  trans  '
```

- Hold `mo2` and press the letter printed with the symbol.
- `&out OUT_USB` / `&out OUT_BLE` force the report channel.
- `&none` keys are intentionally dead.

## Corne — layer 3 · `layer_3` · FN

```text
L: trans  F1   F2   F3   F4   F5             F6   F7   F8   F9   F10  F11
   trans  trans  sys_reset  bootloader       bootloader  PSCRN  F12
   trans  trans  trans  trans  trans         SLCK  PAUSE_BREAK
                   trans trans trans           trans trans trans
```

- Function keys `F1`–`F12`, `PSCRN`, `SLCK`, `PAUSE_BREAK`.
- `&sys_reset` (restart + screen refresh), `&bootloader` (UF2 mode), `&soft_off` (deep sleep).
- Mouse: `&mkp LCLK|MCLK|RCLK|MB4|MB5` and `&mmv MOVE_*`.
- Access pattern: hold a thumb key (`&lt 3 …`) and press `Q` → `F1`.

## Sofle — 3 layers

- **Layer 0** (`LAYER0`): number row + QWERTY, `ESC`, `TAB`, `CAPS`, `C_MUTE`, arrows in the middle, `LALT`, `LGUI`. Left thumb `&mo 1`, right thumb `&mo 2`.
- **Layer 1**: `F1`–`F12`, `LCLK`/`MCLK`/`RCLK`/`MB4`/`MB5`, `PG_UP`/`PG_DN`/`HOME`/`END`/`INS`, arrows, RGB controls, `-`/`=`, `[`/`]`, `C_MUTE`; encoder scrolls the wheel.
- **Layer 2**: `&bt BT_SEL 0`…`&bt BT_SEL 4`, `&bt BT_CLR`, `&bt BT_CLR_ALL`, `&out OUT_USB`/`OUT_BLE`, `&sys_reset`, `&soft_off`, `&bootloader`, `F6`–`F12`, mouse move/clicks, `{`/`}`, `+`/`-`, `~`.

The shipped Sofle bundle has no layer-3 picture.

## Images available in this folder

The DYA STUDIO screenshots used elsewhere in this folder are in `images/`:

| File | Shows |
| --- | --- |
| `images/studio-reset-button.png` | Power (slide) and reset (press) switches on the side of the case |
| `images/studio-connect.png` | DYA STUDIO connect screen (click the USB icon) |
| `images/studio-encoder.png` | Rotary encoder config with a 50 ms tap time |
| `images/studio-modifiers.png` | Modifier checkboxes (LCtrl … RGui) |
| `images/keys-pcb-wifi-icon.png` | Left half display showing the wireless icon |
| `images/display-screen.jpg` | Full display layout (battery, profiles 1–5, LAYER0) |
