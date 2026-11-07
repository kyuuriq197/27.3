# Restore ramdisk report — iOS 27.3

## Verified from target manifest and archive

| Restore identity | Restore behavior | Ramdisk entry | Bytes | SHA-256 |
|---|---|---|---:|---|
| Developer Erase Install (IPSW) | Erase | `141-93054-032.dmg` | 245,366,811 | `eb77af17f7d309cb1db62038ae9d1486d234d0726444920daafd8ebfd21c884e` |
| Developer Upgrade Install (IPSW) | Update | `141-92811-032.dmg` | 247,463,963 | `8289d052a06d2547515bf764309541d06d150c39d47e0403746698355c534eac` |

The manifest maps the restore identities to these archive entries. `ipsw extract --dmg rdisk` extracted both entries, and their full SHA-256 values match the ZIP inventory.

## Windows IM4P parsing

Tool: `blacktop/ipsw` v3.1.743 on Windows x86_64. The exact reproducible command sequence is implemented in `tools/windows/Run-RamdiskMetadata.ps1`; transcript: `research/ios-27.3/logs/ramdisk-metadata-run.txt`.

| Variant | IM4P type | Encrypted | Payload bytes | Payload SHA-256 |
|---|---|---:|---:|---|
| Erase | `rdsk` | false | 245,366,784 | `a7e94c9bec8646c0611fb4c0dd685d369888d6f1310ac37dfde27b7c96d82645` |
| Upgrade | `rdsk` | false | 247,463,936 | `33f7a01ae7808668fdc5c8a3f6b80dc37a810613cb349a80a29e8edcd7588e31` |

`ipsw img4 im4p info --json` reports no compression, `encrypted: false`, and payload lengths equal to those extracted. No key lookup, decryption, or keybag extraction was used.

## Read-only APFS inventory

The extracted payloads were parsed as APFS with the pinned Rust `apfs` 0.4.1 reader. It recognized the volume as `ramdisk`; file, directory, and symlink counts matched the volume's declared counts for both variants. A second implementation, `hadris-apfs` 3.0.0-rc.3, independently traversed each directory tree and agreed on the reachable path sets and entry kinds, with zero traversal errors.

| Variant | Files | Directories | Symlinks | Total entries | APFS-reported file bytes | Hadris size disagreements |
|---|---:|---:|---:|---:|---:|---:|
| Erase | 1,260 | 458 | 306 | 2,024 | 1,237,756,512 | 1,551 |
| Upgrade | 1,519 | 585 | 307 | 2,411 | 1,243,508,278 | 1,810 |

The readers' size fields disagree on many path records, so the byte totals above are output from `apfs 0.4.1` and are not independently validated. The inventories establish filesystem metadata and reachable names, not file-content authenticity, restore behavior, or a security finding. This is direct read-only image parsing, not a filesystem mount.

Reproduce with `tools/windows/Run-RamdiskApfsInventory.ps1`. The runner verifies both payload hashes against the saved extraction summary, checks that APFS file-kind totals match each volume's declared counts, confirms that both readers agree on paths/kinds, and records the size discrepancies. Machine summary: `research/ios-27.3/ramdisk/apfs_inventory_summary.json`; raw listings remain local under `research/ios-27.3/data/ramdisk/apfs-probe/`.

## Machine outputs and limitations

- `research/ios-27.3/ramdisk/ramdisk_metadata_summary.json`
- `research/ios-27.3/ramdisk/141-93054-032.dmg.im4p-info.json`
- `research/ios-27.3/ramdisk/141-92811-032.dmg.im4p-info.json`
- `research/ios-27.3/ramdisk/apfs_inventory_summary.json`
- Raw extracted files and payloads are local-only under `research/ios-27.3/data/ramdisk/`.

No restore utility binaries or behavior have been analyzed. No AEA keys or payload decryption were used.
