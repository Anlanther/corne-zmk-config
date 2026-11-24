# Owner typing profile (ergonomics)

The owner types **90–100 WPM and rolls keys**, so a second key often lands
while a home-row mod is still undecided. They also **chord home-row mods on
purpose** — holding `A` + `S` and then a right-hand letter for a Shift+Ctrl-style
command. The current hold-taps exist to make that combination work; treat this
profile as the reason for the tuning, not as an optional preference.

What that speed means for the timings:

| Speed   | Keys/second | Mean gap between keys |
| ------- | ----------- | --------------------- |
| 90 WPM  | ~7.5        | ~133 ms               |
| 100 WPM | ~8.3        | ~120 ms               |

(5 chars/word × WPM ÷ 60. Real intervals vary, so treat these as means.)

What the profile requires of any keymap change:

1. **Keep `flavor = "balanced"` + `hold-trigger-on-release`.** A hold only wins
   if the interrupting key is still down when the mod key is released, so a quick
   cross-hand roll stays a tap. Never switch to `hold-preferred` (any interrupt
   inside `tapping-term-ms` becomes a stuck modifier). Use the same flavour for
   any _new_ hold-tap instead of bare `&lt` / `&mt`.
2. **Don't retune the timings without evidence at 90–100 WPM.** The current
   values are consistent with that range:
   - `require-prior-idle-ms = 175` sits above the ~120–133 ms mean gap with
     ~42–55 ms of headroom for slower keystrokes, so sustained typing forces taps
     without relying on the owner never slowing down. Intentional holds pay for
     this with a ~175 ms pause before the hold key. Lower it only if deliberate
     holds feel laggy; raise it only for a reproducible accidental hold.
   - `tapping-term-ms = 200` is longer than any typing hold, so only deliberate
     holds reach it.
   - `quick-tap-ms = 175` is above the mean gap, so fast same-key repeats ("ll",
     "ee") still resolve as taps.

   Raising `require-prior-idle-ms` makes holds harder to trigger on _every_ key, so
   only do it for a reproducible misfire, and never below ~150 ms.

3. **Keep the `hold-trigger-key-positions` hand-gating.** `hm` holds only from
   opposite-hand/thumb keys (`RIGHT_KEYS THUMB_KEYS`), `thumb_lt` from the right
   hand, `thumb_lt_own` from the left hand/thumb. Same-hand rolls must never
   resolve a mod. Consequence: a same-hand chord like `A`+`S` only becomes two
   modifiers once an opposite-hand key is pressed — which is exactly the owner's
   chording pattern. Two things must both hold for that chord: the first key needs
   a ≥175 ms pause before it (the idle gate), and the right-hand key has to land
   while both mods are still down. Never remove the gating to "fix" chording.
4. **Never place a combo on a chorded home-row mod pair.** Positions `14`/`15`
   (`A`/`S`) and the other home-row mods are pressed _simultaneously_ as
   modifiers; a combo there would fire instead of the chord. Check `key-positions`
   of any new combo against `1 15 29` (soft off) and the home-row pairs.
5. **Report what you verified.** Any change touching hold-taps, the home row or
   combos should say in the summary that flavour, timings, hand-gating and
   combo positions are unchanged (or explain why they had to change).

If a symptom still appears at speed (a tap becomes a hold, a chord drops a mod),
diagnose it against this profile before reaching for a different flavour.
