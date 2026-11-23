---
name: updating-layers
description: Edit or add keyboard layers in this Eyelash Corne ZMK repo, driving the change from the ASCII layer diagrams in README.md. Use when the user wants to rebind a key, move a thumb key, add/rename/reorder a layer, or make a layer the new default — includes generating a new README layer diagram, keeping config/eyelash_corne.keymap and the keymap header comment in sync, and validating every layer against the README.
---

# Updating layers from the README

**The ASCII diagrams in `README.md` are the source of truth for what each layer
binds.** `config/eyelash_corne.keymap` is that truth rendered into devicetree.
When the two disagree, the README wins and the keymap follows — never the other
way round, and never silently.

## 1. Before editing — read and lock the current state

1. Read `README.md` (layer diagrams) and `config/eyelash_corne.keymap`.
2. Read the position map and binding rules in `reference-docs/layers.md` +
   `reference-docs/custom-behaviours.md` **only if** you need keycode syntax or a
   behaviour the repo does not already define. `AGENTS.md` → "Rules for editing
   this repo" overrides anything there.
3. Run the layer check (section 5) **before** changing anything. If it already
   reports drift, tell the user in one line and fix that drift as part of the
   work — do not build a new discrepancy on top of an old one.
4. `.conf` and the shield DTS are out of scope for layer edits. If the request
   needs a `CONFIG_*` change, that is a separate edit to
   `config/eyelash_corne.conf`.

## 2. Position map — how a cell in the diagram maps to a binding

The keymap is one flat list per layer in **keymap order**, 48 entries. Column N of
a diagram row is binding N of that layer. Current Base layer, for reference:

| Diagram area     | Positions  | Order                             |
| ---------------- | ---------- | --------------------------------- |
| Row 0, left      | `0–5`      | `0 1 2 3 4 5`                     |
| Stick up (row-0) | `6`        | `UP`                              |
| Row 0, right     | `7–12`     | `7 8 9 10 11 12`                  |
| Row 1, left      | `13–18`    | `13 14 15 16 17 18`               |
| Stick mid        | `19 20 21` | `LEFT` `ENTER` `RIGHT`            |
| Row 1, right     | `22–27`    | `22 23 24 25 26 27`               |
| Row 2, left      | `28–33`    | `28 29 30 31 32 33`               |
| Row 2, right     | `36–41`    | `36 37 38 39 40 41`               |
| Thumbs           | `42–47`    | left `42 43 44`, right `45 46 47` |

The halves are **not** offset symmetrically: left is `0–5 / 13–18 / 28–33`, right
is `7–12 / 22–27 / 36–41`. The stick keys are `6` (`UP`), `19` (`LEFT`), `20`
(`ENTER`), `21` (`RIGHT`); the two `-` cells between the halves are `34` and `35`.
Re-derive this as index/binding pairs from layer 0 before relying on it if the
keymap changed shape, rather than trusting the table blindly.

Two consequences of the flat layout:

- Inserting a layer in the middle of `keymap { }` **shifts every
  `&thumb_lt N` / `&mo N` / `&tog N` index** at or after it. Recount all of them.
- `hold-trigger-key-positions` uses **keymap positions**, not matrix positions
  (see `AGENTS.md` and the `RIGHT_KEYS` / `THUMB_KEYS` macros). Adding a layer
  does not change them, but moving a thumb key or adding a hand does.

## 3. Classify the request

### A. Rebind or move existing keys

1. Edit the ASCII cell(s) in the relevant README layer diagram **first**. Keep
   the box widths consistent; `text` blocks are read by humans, so padded columns
   must stay aligned. Update the `Legend`/prose above the diagram if a new kind of
   binding appears.
2. Then edit the matching binding in the corresponding keymap layer. Keep the
   file's visual row alignment (several bindings per line, column-aligned with the
   diagram).
3. Update the keymap header comment if the alpha/thumb summary changed.

### B. Add a new layer

The README diagram comes first, always. Steps:

1. **Ask for the trigger.** A layer nobody can reach is a bug. If the user has not
   said how it is entered, ask once (which thumb key / which key holds it) rather
   than inventing one. Never wire a new layer to a key the user didn't name.
2. **Write the diagram** under a new `### Layer N — <Name>` heading, matching the
   existing style exactly: `LEFT HALF` / `RIGHT HALF` labels, `+---+` borders, the
   `THUMB KEYS` block, and the `THUMB STICK` block.
3. **Fill unassigned cells with `-`** (transparent) — see the fill rule below.
4. **Append the layer node to the end of `keymap { }`** unless the user asked for a
   specific position. The **first node is always the default layer**, so appending
   never changes the default and keeps every existing `&thumb_lt N` / `&mo N`
   index stable. The `N` in the README `### Layer N` heading is the _display order_
   and must equal the node's index in `keymap { }`: number a new appended layer
   one higher than the last existing layer.
5. Add a binding that activates it (`&thumb_lt N <tap>`, `&hm`, `&mo N`, …) to the
   layer the user named, in both the diagram and the keymap, and update the
   `held from` line under the owning layer's heading.
6. Every layer gets `sensor-bindings = <&rsr_vol &rsr_trans>;` unless the user
   explicitly wants different encoder behaviour on that layer.

**Fill rule for a new layer:**

- Default: every cell the user did not specify is `&trans` / `-` in the README, so
  the layer inherits Base. Nothing else.
- If the user specifies bindings, use exactly those and `&trans` for the rest.
- Only a layer that is **the new default layer** gets full content; a blank
  default layer would remove access to everything.
- `&none` / `(none)` is only for a cell the user explicitly wants dead on that
  layer — it blocks inheritance, so never use it as a filler.

**Blank diagram template.** Copy this into `README.md` and replace the `-` cells
with the layer's bindings. The grid is 98 characters wide, identical to the
Layer 0–2 diagrams: 6 cells of 7 chars, a `++` half divider, 6 cells of 7 chars.
Keep the borders aligned; the `N`/`Name`/`held from` line and the thumb rows must
be filled in for the specific layer.

````text
### Layer N — Name

Held from <where>.

```text
LEFT HALF                                            RIGHT HALF
+-------+-------+-------+-------+-------+-------++-------+-------+-------+-------+-------+-------+
| -     | -     | -     | -     | -     | -     || -     | -     | -     | -     | -     | -     |
+-------+-------+-------+-------+-------+-------++-------+-------+-------+-------+-------+-------+
| -     | -     | -     | -     | -     | -     || -     | -     | -     | -     | -     | -     |
|       |       |       |       |       |       ||       |       |       |       |       |       |
+-------+-------+-------+-------+-------+-------++-------+-------+-------+-------+-------+-------+
| -     | -     | -     | -     | -     | -     || -     | -     | -     | -     | -     | -     |
+-------+-------+-------+-------+-------+-------++-------+-------+-------+-------+-------+-------+

THUMB KEYS
Left:  -                  | -                | -
Right: -                  | -                | -

THUMB STICK
          UP
LEFT - (none) - RIGHT
         DOWN
````

Mapping the template to positions: the top-right cell is position `12`, the
middle row's `LEFT`/`(none)`/`RIGHT` cells are `19`/`20`/`21`, the bottom-left cell
is `28`, the bottom-right cell is `41`, and the six thumb cells are `42`–`47` in
reading order (see the position map in section 2).

### C. Make a layer the new default

The default layer is **position 0** in `keymap { }`, not something a behaviour
turns on. Moving a layer to the bottom reindexes everything, so:

1. Reorder the layer nodes in `keymap { }` (the first node is the default).
2. Renumber every layer index in every `&thumb_lt` / `&mo` / `&tog`, plus the
   `Layer numbering:` line and the `Number layer, left half:` block in the header.
3. Reorder and renumber the README `### Layer N` headings so README order ==
   `keymap { }` order, and fix every `held from` reference.
4. Call out that the old default's bindings are now only reachable by holding its
   trigger, and check that nothing became unreachable.

### D. Rename / reorder only

Touch the README heading, the `display-name` in the keymap, the prose that names
the layer, and (for reorder) every layer index as in C. A rename of
`display-name` changes what the nice!view shows — mention it.

## 4. Behaviour vocabulary — use what the repo already defines

| Intent                                     | Use                                           | Not                                      |
| ------------------------------------------ | --------------------------------------------- | ---------------------------------------- |
| mod on hold, key on tap (home row, thumbs) | `&hm <MOD> <KEY>`                             | `&mt`                                    |
| layer on hold, key on tap                  | `&thumb_lt <layer> <KEY>`                     | `&lt`                                    |
| momentary layer, no tap                    | `&mo <layer>`                                 | bare `&mo` on a thumb — you lose the tap |
| shift+a letter                             | `&kp LS(A)`, or `&hm LSHFT A` if it is a hold |
| dead cell                                  | `&none`                                       | `&trans` (which inherits)                |
| inherit                                    | `&trans`                                      | omitting the binding (breaks the count)  |

Modifier keycodes in this repo's style: `LSHFT LCTRL LALT LGUI RSHFT RCTRL RALT
RGUI`, combined as `LC(...) LS(...) LA(...) LG(...)`. Diagram legend: `Ctrl`,
`Shift`, `Alt`, `Cmd`, `key / hold` for hold-taps, `-` for transparent,
`(none)` for `&none`.

## 5. Validate before finishing — mandatory

1. **Count and compare.** Every layer must have exactly 48 bindings (no more, no
   fewer), and the N-th binding must be what the N-th cell of the README diagram
   says. Do this mechanically, not by eye: parse the `bindings = < … >;` blocks
   out of the keymap, translate each to its README spelling (`&kp Q` → `Q`,
   `&hm LSHFT A` → `A` over `Shift`, `&thumb_lt 2 ESC` → `ESC` (hold: that layer's
   name), `&trans` → `-`, `&none` → `(none)`), and diff against a transcription of
   the README. The README already states each thumb hold by name
   (`ESC (hold: Number)`), so compare names, not raw indexes. Use the repo's own
   script if one exists; otherwise write a throwaway one, run it, paste its output
   in your summary, and **delete it** (do not leave helper files in the tree).
2. **Check reachability.** Every layer other than the default must be triggered by
   at least one binding, and every `&thumb_lt N` / `&mo N` must point at a layer
   that exists.
3. **Check hold-taps.** Any new `&hm` / `&thumb_lt` usage needs the right
   `hold-trigger-key-positions` group (`RIGHT_KEYS`, `THUMB_KEYS` — keymap
   positions). A thumb that holds a layer must react to the opposite hand.
4. **Check the header comment** in `config/eyelash_corne.keymap` against the final
   bindings (alpha rows, thumb summary, layer numbering, Number-layer block).
5. **Check other docs** that name layers or bindings: `README.md` prose
   (`thumb scroller`, `Notes`, the Bluetooth legend), `AGENTS.md` → "Known
   divergences" if the layer count/names changed, and
   `.agents/skills/reference-docs-navigation/SKILL.md` if it lists the layers.
   Do not edit `reference-docs/`.

## 6. Report gaps, don't silently pad

Finish by telling the user, briefly:

- what changed, file by file (README diagram → keymap layer → header comment →
  other docs);
- the validation output (all layers, 48/48, matching);
- **integral bindings missing from every layer**, if any. Check at least:
  a way to reach _every_ non-default layer; soft-off (`Q+S+Z` combo) if that combo
  is still valid after moving keys; Bluetooth/output control (currently only the
  Number layer, left hand); Caps Lock; a place for `RGB_TOG`/`RGB_ON`/`RGB_OFF` if
  underglow is expected to be controllable; media transport; and any key the user
  just removed and did not re-place anywhere. If a binding exists on no layer at
  all, say so and propose where to put it — do not add it unasked.
- whether both halves need re-flashing (keymap changes are compiled into each
  image; the right half only needs a flash if its bindings changed, but the
  hold-tap/flavour config is shared, so say explicitly what you recommend).

```

```
