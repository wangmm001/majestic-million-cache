# majestic-million-cache

Daily snapshots of the [Majestic Million](https://majestic.com/reports/majestic-million) top-1M domain list, mirrored by [`wangmm001/feedcache`](https://github.com/wangmm001/feedcache).

## Layout

```
data/
  YYYY-MM-DD.csv.gz     # daily snapshot, gzipped
  current.csv.gz        # pointer to the most recent snapshot
```

File contents are the upstream `majestic_million.csv` verbatim — all **12 columns** preserved:

```
GlobalRank, TldRank, Domain, TLD, RefSubNets, RefIPs, IDN_Domain, IDN_TLD,
PrevGlobalRank, PrevTldRank, PrevRefSubNets, PrevRefIPs
```

Unlike other top-million lists that only track popularity, Majestic's value is the link-graph columns (`RefSubNets`, `RefIPs` = number of referring subnets / IPs), which persist here in full.

## Consume

```bash
# latest only
curl -L https://raw.githubusercontent.com/wangmm001/majestic-million-cache/main/data/current.csv.gz | zcat | head

# full history
git clone --depth=1 https://github.com/wangmm001/majestic-million-cache.git
```

## License

The Majestic Million dataset is provided free-of-charge by Majestic-12 Ltd. under [Creative Commons BY 4.0](https://creativecommons.org/licenses/by/4.0/) — attribute: "Majestic Million by Majestic (majestic.com)". See [Majestic's page](https://majestic.com/reports/majestic-million) for current terms.

The README and cron scaffolding in this repo are MIT.

## How it works

Daily GitHub Actions cron (04:15 UTC) calls `wangmm001/feedcache`'s reusable workflow, which downloads `https://downloads.majestic.com/majestic_million.csv` (~80 MB) and gzips it. No commit is produced when content hasn't changed.
