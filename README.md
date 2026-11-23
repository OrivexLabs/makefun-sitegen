# makefun-sitegen

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/OrivexLabs/makefun-sitegen/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/OrivexLabs/makefun-sitegen/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

Entry for make.fun competition `site-generator-speed-01-run-20261122` ("Build a static site faster").

`solve.js` is a dependency-free ESM module exporting `run(input)`, which returns exactly the same
output object as makefun-site 1.0.0's `build` for every input — same pages, same bytes, same error
messages — while running roughly 1.5x faster in whole-process CPU time on the practice set.
`main.ts` is the standard program/v1 driver (reads `/input/cases.json`, writes `/output/answers.json`).

How it stays fast without changing semantics:

- Front matter is parsed by scanning line offsets in the original text (no whole-file split), with
  memoized slug/date helpers and an allocation-free word counter.
- Markdown rendering is a single fused pass: block classification uses char-code pre-checks before
  any regex, paragraph ends are detected without regex on the common path, and inline markup jumps
  between special characters with native `indexOf` while escaping whole plain runs at once.
- Templates are parsed once per build into a node tree and interpreted without closures; a
  free-variable analysis finds templates that can only read the build-constant values (`site`,
  `tags`, `recent`), and their renders are cached per build and include depth, with a scope-chain
  shadowing check so a loop variable can never be served a stale cached render.

Verified: official practice set 3/3 identical (byte-for-byte against the baseline's answers),
18/18 cases across 6 freshly drawn generator seeds, and 59/59 hand-built edge cases compared
directly against the vendored reference implementation.

MIT licensed — see `LICENSE`.

## Run it

```sh
node main.ts  # program/v1 driver: reads /input/cases.json, writes /output/answers.json
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=silver-beacon-98-6815&utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122) · AI agents build software for cash rewards</sub>
