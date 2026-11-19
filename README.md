# makefun-token-radar-01

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/rikonprdy/makefun-token-radar-01/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/rikonprdy/makefun-token-radar-01/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

A token launch radar: it reads a sealed DefiLlama snapshot (758 protocols, 14 November 2026), ranks every pre-token protocol by an implied value, and hands the reader a dashboard with a board, a detail view per protocol, filters, and a farm plan for each row.

## The model (radar-2)

`implied = round(tvl_usd × category_multiple × growth × maturity)`

- **Category multiple** — the market's own price for a dollar of TVL in the protocol's category: the median market-cap-to-TVL ratio of the live-token protocols in that category that still carry a positive market cap, shrunk toward the market-wide median (0.214×) by five pseudo-peers. A category with fewer than three live protocols trading takes the market median outright: one record is not a market. The dashboard shows the full calibration table.
- **Growth** — `1 + clamp(change_7d, −60, 120) / 100 × 0.5`.
- **Maturity** — `0.6 + min(age_days, 1095) / 1095 × 0.4`, where age is the time since the protocol was first listed (365 days when unlisted).

The board covers all 40 watchlist protocols plus 158 more rankable pre-token protocols from below the floor. Excluded from the board entirely: exchanges, bridges, liquid staking and restaking products, wrapped or staked products, and versioned pools — their tokens already trade, so they are not launches waiting to happen. Live-token protocols are set aside in their own view with their market caps.

Top of the board: Steakhouse Financial ($464.9M implied), Sentora Curator ($446.6M), Gauntlet ($276.9M).

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json>
```

Reads the snapshot, writes `board.json`, `data.js`, `dashboard/`, `screenshots/` and `dossier.json` to the output directory. Node 22, standard library only, offline, deterministic — two runs write the same bytes.

---

<sub>Entered on [make.fun](https://make.fun/?ref=wpdqfrv1&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
