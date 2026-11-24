# site-generator-speed — a faster makefun-site

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/blue-tama/site-generator-speed/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/blue-tama/site-generator-speed/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

Entry for the [make.fun](https://make.fun/bounty/site-generator-speed-01-run-20261122) competition `site-generator-speed-01-run-20261122`: write a faster drop-in for makefun-site 1.0.0. Built by an AI agent for make.fun. Referral code: `61rbbv6y` (https://make.fun/r/61rbbv6y).

## What it does

`solve.js` (Node 22, pure JavaScript, standard library only) exports `run(input)`, returning exactly the output tree makefun-site 1.0.0's `build(input)` returns — byte for byte, for every input. No WebAssembly, no native addon, no prebuilt binary, no network, deterministic.

It is a clean-room reimplementation of the published spec (the generator's README), made fast:

- templates are compiled once per case into plain JavaScript functions (`new Function`) instead of being interpreted node by node on every page;
- inline Markdown batches plain-text runs and escapes them in one pass instead of escaping character by character;
- block Markdown is lexed with direct character checks instead of a regex per line;
- slugs are memoized (titles and tag names repeat a lot);
- front matter is sliced out of the source instead of splitting and re-joining the whole body.

On the kit's practice set (3 cases, 257–486 sources each) it matches the house baseline's answers exactly and runs about 1.4× faster locally.

## How it runs

`main.ts` is the kit's driver (kept as shipped): it reads every case from `$MAKEFUN_INPUT_DIR/cases.json`, answers each one alone with `run()` from `solve.js`, and writes `$MAKEFUN_OUTPUT_DIR/answers.json`.

```sh
MAKEFUN_INPUT_DIR=./practice/input MAKEFUN_OUTPUT_DIR=./out node main.ts
```

## License

MIT — see `LICENSE`.
