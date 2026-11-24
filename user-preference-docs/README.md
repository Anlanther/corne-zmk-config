# User preference docs

Owner-specific working preferences and ergonomics notes for this repo. They are
separated from the project-wide agent guide in `AGENTS.md` so the technical guide
can be reused or shared while these stay personal.

## How agents should use this folder

- If this folder exists, read it before acting on the relevant task and treat it
  as higher priority than the general defaults in `AGENTS.md`.
- If it is absent, fall back to `AGENTS.md` and the general repo guidance.
- `qmk-reference.md` is a **record only**. It documents a different keyboard (a
  ZSA Voyager running QMK); never follow its key bindings or port them directly.
- It never overrides the ground truth: `config/eyelash_corne.keymap`,
  `config/eyelash_corne.conf`, `build.yaml`, `config/west.yml` and the shield
  hardware still win.

## Contents

| File                | What it covers                                                           |
| ------------------- | ------------------------------------------------------------------------ |
| `working-style.md`  | How the owner wants the agent to communicate and scope work.             |
| `typing-profile.md` | The typing/ergonomics profile that motivates the hold-tap tuning.        |
| `qmk-reference.md`  | Record of the owner's ZSA Voyager QMK layout — **not** a binding source. |
