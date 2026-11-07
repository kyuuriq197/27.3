# Trustcache metadata report — iOS 27.3

## Method and validation

`tools/windows/Run-TrustCacheSummary.ps1` ran `ipsw fw tc <IPSW> --json` using Windows x86_64 `blacktop/ipsw` v3.1.743. The parser output named five trustcache components. Each path was matched to the local ZIP inventory; the parser-reported declared entry count was compared to the parsed entry array.

No individual trustcache hashes are copied into the portable summary/report. The full parser JSON, which contains individual hashes, is retained only under ignored local `research/ios-27.3/data/trustcache/`. No key lookup, decryption, or vulnerability analysis was performed.

| Archive entry | Version | UUID | Entries | Duplicate hashes |
|---|---:|---|---:|---:|
| `Firmware/141-92811-032.dmg.trustcache` | 1 | `FBA5EEF9-41FF-4D70-A765-651B0280C331` | 378 | 0 |
| `Firmware/141-93054-032.dmg.trustcache` | 1 | `22DC2C6B-1C7A-403C-B92C-31348B8286A7` | 371 | 0 |
| `Firmware/141-93238-032.dmg.trustcache` | 2 | `7D8AF115-ECC8-4CA9-A11C-BA7B9575DACE` | 34 | 0 |
| `Firmware/141-93752-026.dmg.aea.trustcache` | 2 | `07977E62-F89B-425F-B889-4D1052918419` | 3,840 | 0 |
| `Firmware/141-94047-030.dmg.aea.trustcache` | 2 | `3896539E-AFE9-40D4-9937-A48CC6724C76` | 131 | 0 |

Total declared and parsed entries: **4,754**. The parser-reported hash type was SHA-256 for all five caches.

## Limits

These are code-signing trustcache inventory facts only. They do not prove which binaries are present in an encrypted volume, establish runtime behavior, or indicate a vulnerability. They are not compared to 27.2 because a corresponding complete old trustcache inventory was not available.

## Cross-cache overlap

The five parsed caches were compared locally using their code-signing hashes. The analysis found 4,329 unique hashes across 4,754 total cache memberships, with 425 memberships beyond each hash's first occurrence. Pairwise overlap counts are recorded in [trustcache-overlap-report.md](trustcache-overlap-report.md). The raw hash values were not added to portable outputs.

Overlap counts describe metadata membership only. They do not show which binaries are present or executed, and they are not vulnerability findings.

## Evidence

- `research/ios-27.3/trustcache/trustcache_summary.json`
- `research/ios-27.3/trustcache/trustcache_overlap_summary.json`
- `research/ios-27.3/trustcache/trustcache-overlap-report.md`
- `research/ios-27.3/logs/trustcache-summary-run.txt`
- Full raw parser JSON: local-only at `research/ios-27.3/data/trustcache/trustcache-parser-output.json`
