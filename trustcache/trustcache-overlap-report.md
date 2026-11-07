# Trustcache overlap summary - iOS 27.3

Compared the five parsed trustcaches from build `24C5037i` by their code-signing hashes. Hash values are used transiently for set comparisons and are not written to this report or its machine-readable summary.

- Trustcaches: 5
- Entry memberships: 4754
- Unique hashes across all caches: 4329
- Repeated memberships beyond the first occurrence: 425

| First trustcache | Second trustcache | Shared hashes |
|---|---|---:|
| `Firmware/141-92811-032.dmg.trustcache` | `Firmware/141-93054-032.dmg.trustcache` | 370 |
| `Firmware/141-92811-032.dmg.trustcache` | `Firmware/141-93238-032.dmg.trustcache` | 0 |
| `Firmware/141-92811-032.dmg.trustcache` | `Firmware/141-93752-026.dmg.aea.trustcache` | 53 |
| `Firmware/141-92811-032.dmg.trustcache` | `Firmware/141-94047-030.dmg.aea.trustcache` | 2 |
| `Firmware/141-93054-032.dmg.trustcache` | `Firmware/141-93238-032.dmg.trustcache` | 0 |
| `Firmware/141-93054-032.dmg.trustcache` | `Firmware/141-93752-026.dmg.aea.trustcache` | 51 |
| `Firmware/141-93054-032.dmg.trustcache` | `Firmware/141-94047-030.dmg.aea.trustcache` | 2 |
| `Firmware/141-93238-032.dmg.trustcache` | `Firmware/141-93752-026.dmg.aea.trustcache` | 0 |
| `Firmware/141-93238-032.dmg.trustcache` | `Firmware/141-94047-030.dmg.aea.trustcache` | 0 |
| `Firmware/141-93752-026.dmg.aea.trustcache` | `Firmware/141-94047-030.dmg.aea.trustcache` | 0 |

These are overlap counts in trustcache metadata only. They do not prove which binaries are present in an encrypted volume, establish runtime behavior, or indicate a vulnerability.

Machine-readable result: `research/ios-27.3/trustcache/trustcache_overlap_summary.json`.
