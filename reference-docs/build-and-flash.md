# Repository Layout & Build

## What you get when you fork

A forked Corne config repo is a standard ZMK _user config_ repository:

```text
<repo>/
├── .github/
│   └── workflows/
│       └── build.yml          # GitHub Actions: builds every keyboard in build.yaml
├── build.yaml                 # which shield+board combos to build (left, right, settings_reset, dongle)
├── config/
│   ├── west.yml               # ZMK + module revisions to pull in
│   ├── corne.keymap           # ← the file you edit for key changes
│   ├── corne.conf             # global CONFIG_* options
│   ├── corne_left.conf        # left-half-only overrides
│   ├── corne_right.conf       # right-half-only overrides
│   ├── corne.dtsi             # matrix, kscan, sensors, split transform
│   ├── corne_left.dts         # central role, display, default layer setup
│   ├── corne_right.dts        # peripheral role
│   └── boards/                # optional custom board/shield definitions
└── README.md                  # layer diagrams (this is what the product page links to)
```

File names vary slightly between the ink-screen, OLED and dongle repos, and the dongle repo adds a `dongle` shield (usually built with `-DEXTRA_CONF_FILE`/`boards/shields`). Always check `build.yaml` to see the exact artifact names.

## Editing workflow

1. **Fork** the correct repository (see [index](index.md#quick-facts)). Never edit someone else's fork.
2. Create a branch for your change (GitHub UI or locally).
3. Edit `config/corne.keymap` (99% of changes) — see [layers](layers.md).
4. Commit. GitHub Actions builds automatically.
5. In **Actions**, open the run and download the artifact(s) at the bottom.

### If you build locally

```sh
# one-time
west init -l config
west update
west zephyr-export

# build every target listed in build.yaml
west build -s zmk/app -d build/left \
  -b nice_nano_v2 -- \
  -DZMK_CONFIG="$PWD/config" \
  -DSHIELD="corne_left nice_view"

west build -s zmk/app -d build/right \
  -b nice_nano_v2 -- \
  -DZMK_CONFIG="$PWD/config" \
  -DSHIELD="corne_right nice_view"
```

`nice_nano_v2` is the board name for the `52840micro` board; `nice_view_adapter` may need to be prepended to the shield list on boards where the display sits on a separate adapter. Adjust the shield names to match your repo's `build.yaml`.

## Choosing the right artifact

The build produces one `.uf2` per target. Match by **shield name + side**:

| Suffix                         | Goes on                                                                     |
| ------------------------------ | --------------------------------------------------------------------------- |
| `…_left.uf2` / `*studio_left*` | left half                                                                   |
| `…_right.uf2`                  | right half                                                                  |
| `…central_dongle…uf2`          | USB receiver                                                                |
| `settings_reset…uf2`           | either half (clears pairing) — flash both halves, and the dongle if present |

Examples seen in the shipped bundles: `eyelash_corne_studio_left.uf2`, `eyelash_corne_right nice_view-nice_nano_v2-zmk.uf2`, `eyeslash_corne_central_dongle_oled.uf2`. Always confirm the exact artifact names against your repo's `build.yaml`.

## Flashing procedure (both halves)

1. Connect the half with a **data** USB cable (charge-only cables are the #1 cause of "no drive appears").
2. Double-click **reset** within 0.5 s → a drive named `nicenano` mounts.
3. Copy **one** `.uf2` to the drive root. The drive unmounts itself when the flash completes (macOS may show an error — ignore it).
4. Never delete files from the drive; never copy several `.uf2` at once.

**Order for a full reset / re-pair:**

```text
left  ← settings_reset   (wait for reboot)
right ← settings_reset   (wait for reboot)
left  ← corne_left.uf2
right ← corne_right.uf2
```

Pairing between halves is stored in flash, so a keymap-only change needs **only the left half** re-flashed. On dongle builds the keymap lives on the receiver, so flash the `central_dongle` image.

## Version control hygiene

- Keep one change per branch so a bad build is easy to revert (a red × build = revert the commit and re-run, or re-fork).
- Don't rename `corne.keymap`/`corne.conf` — the shield name in `build.yaml` must match.
- If you add a new behaviour block, put it **above** the `keymap { … }` node in the same `.keymap` file (ZMK's DTS preprocessor resolves `&references` at parse time).
