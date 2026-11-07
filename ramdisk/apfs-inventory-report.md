# Restore ramdisk APFS inventory - iOS 27.3

Both unencrypted restore ramdisk payloads were parsed read-only with `apfs 0.4.1` and independently walked with `hadris-apfs 3.0.0-rc.3`. Payload sizes and SHA-256 values were checked against the previously verified IM4P extraction summary.

| Variant | Files | Directories | Symlinks | Entries | APFS-reported file bytes | Hadris size mismatches |
|---|---:|---:|---:|---:|---:|---:|
| Erase | 1260 | 458 | 306 | 2024 | 1237756512 | 1551 |
| Upgrade | 1519 | 585 | 307 | 2411 | 1243508278 | 1810 |

For each variant, the independent reader found the same reachable path set and entry kinds, with zero traversal errors. Size fields do not independently agree for many entries; byte totals are therefore tool-reported only and are not treated as verified content totals. Local listings and raw parser output are kept under ignored `research/ios-27.3/data/ramdisk/apfs-probe/`.

No AEA keys, decryption, mounting, or vulnerability analysis was performed.

Machine-readable summary: `research/ios-27.3/ramdisk/apfs_inventory_summary.json`.
