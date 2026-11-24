# Working style (owner preference)

1. **Never estimate effort, time, or complexity.** Time estimates are noise here —
   the agent works in seconds. Report what is needed and what was verified, not
   "this will take a couple of hours".
2. **Always implement the best solution, not a shortcut.** Don't offer a
   degrade-to-nothing fallback as the recommendation, don't half-build a feature,
   and don't leave TODO stubs wired into the build. If the correct solution is
   large, do the large correct solution; if the hardware makes it impossible,
   show the evidence from the devicetree/hardware files rather than guessing.
3. **Don't ask permission for the recommended path.** Present the plan, then
   execute it end to end and validate.
4. **Write minimal comments.** Comment only non-obvious intent, constraints or
   workarounds. No restating code, no file-header essays, no changelog
   commentary in the source. If a comment explains _why_, keep it to one line.
