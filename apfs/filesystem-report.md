# Filesystem and APFS report — iOS 27.3

## AppOS APFS inventory — build 24C5037i

The BuildManifest identifies `141-93238-032.dmg` as the AppOS component. It was extracted with the pinned Windows `ipsw` tool; size (18,874,368 bytes) and SHA-256 (`0b549ce47523094c0e55efbbcdc92cbf65782464d1ff442096f2af22b5979f34`) match the ZIP inventory.

The first 4 KiB block is an APFS container superblock (`NXSB`, object type `0x80000001`, object ID 1, transaction ID 15). Its stored Fletcher64 checksum (`0x1686DA2AA6671AD4`) matches independent read-only calculations.

The container has xid 15, 4,608 blocks, and 4 KiB blocks. Its AppOS volume is named `AppCryptex` (volume OID 1026, xid 13), has no snapshots, and advertises incompatibility flags `0x28`, including the sealed-volume bit. The volume's root filesystem tree is a virtual, hashed B-tree whose root OID 1174 maps to physical block 21 with `OMAP_VAL_NOHEADER`.

### Sealed FS-tree walk and independent comparison

A format-aware read-only walker handles the sealed tree's headerless nodes, virtual child OIDs, and SHA-256 child hashes. It found 128 nodes (124 leaves), 6,629 records, and 127 child links; all 127 child hashes and expected child levels matched, with no traversal errors. The counts agree with the tree footer.

The resulting root-directory walk contains 1,365 paths: 1,212 file entries, 153 directories, and no symlink or other entries. Those file paths refer to 896 unique file inodes, so multiple directory entries may refer to the same inode. The root contains `/System` and `/usr`. No inode was missing during the walk.

An independent reader, `hadris-apfs 3.0.0-rc.3`, also opened the volume and walked it. After sorting to remove traversal-order differences, its listing matched the format-aware walk exactly: 1,365 of 1,365 paths, with no path, kind, or known nonzero-size mismatches. It independently resolved selected paths and read two small uncompressed files. This is useful corroboration, but both tools implement reverse-engineered APFS and this is not an Apple reference implementation.

The APFS volume superblock reports 896 files, consistent with the number of unique file inodes; the directory walk emits 1,212 file paths. Sizes are incomplete: only 204 file paths had a nonzero, available data-stream size, while 1,008 did not. Their known sizes sum to 4,800,424 bytes; this is a lower-bound metadata sum, not the volume's total content size.

### Sealed-volume integrity metadata

The volume object map has 129 entries. OID 1172 maps to block 20, where a checksum-valid APFS object of type `INTEGRITY_META` (`0x1e`) was parsed. It reports version 2, hash type SHA-256, root-hash offset 128, and `broken_xid=0`. This accounts for a sealed-volume integrity-metadata object in addition to the 128 FS-tree nodes. OID 1173 has no mapping and remains unexplained.

`broken_xid=0` means this metadata record does not mark the seal broken. The Merkle root at the recorded offset was not extracted or recomputed, and no seal verification is claimed.

### Reader limitations and reproducibility

`ipsw disk dmg` rejected the image as not a DMG. `ferrosys inspect --quick --json` detected APFS but refused the sealed-volume feature (`0x20`). These initial parser refusals were tool limitations; subsequent format-aware tree traversal and the independent `hadris-apfs` read succeeded without mounting or modifying the image.

Reproducible source and results:

- `tools/rust/apfs-probe/src/bin/appos-walk.rs` — sealed-tree traversal, child-level checks, and SHA-256 child-link validation.
- `tools/rust/apfs-probe/src/bin/integrity-meta.rs` — checksum-verified parsing of the integrity-metadata object.
- `tools/rust/hadris-probe/src/main.rs` — independent APFS reader, listing, path resolution, and comparison.
- `research/ios-27.3/apfs/appos_superblock_summary.json`
- `research/ios-27.3/apfs/appos_integrity_meta_summary.json`
- `research/ios-27.3/apfs/hadris_appos_validation.json`
- `research/ios-27.3/logs/appos-rust-walk.txt`

The raw AppOS listings remain local under ignored `research/ios-27.3/data/appos/`.

The independent [apfs-fuse](https://github.com/sgan81/apfs-fuse) reader also documents sealed-volume support, but requires Linux FUSE and was not needed for this read-only cross-check. WSL remains uninstalled; no elevation was requested or performed.

## AEA SystemOS volumes

The large SystemOS and Cryptex SystemOS images are AEA containers. Their metadata fields were identified without saving field values, looking up keys, or attempting decryption. No APFS analysis of those encrypted images is claimed. See `research/ios-27.3/aea/aea-report.md`.

## Root-hash sidecar inventory

The three `.root_hash` files are each 229 bytes. Their archive SHA-256 values were independently recomputed from ZIP streams and matched `firmware/ipsw_inventory.csv`. Literal ASCII markers present in the bytes are:

| Sidecar | Literal markers |
|---|---|
| `Firmware/141-93238-032.dmg.root_hash` | `IM4P`, `casy` |
| `Firmware/141-93752-026.dmg.aea.root_hash` | `IM4P`, `isys` |
| `Firmware/141-94047-030.dmg.aea.root_hash` | `IM4P`, `cssy` |

The pinned `ipsw img4 im4p info --json` parser identifies these as unencrypted IM4P headers, version `0`, with 208-byte payloads and types `casy`, `isys`, and `cssy` respectively. This confirms wrapper metadata only; it does not decode or validate the inner root-hash structure, APFS volumes, or signatures.

Runner: `tools/windows/Run-RootHashInventory.ps1`  
Machine output: `research/ios-27.3/apfs/root_hash_sidecar_inventory.json`

For 27.3, the `.mtree` entry was confirmed as an unencrypted `msys` IM4P wrapper with a `pbze` payload. Its 37 framed LZFSE streams were decoded, but only two bounded Apple Archive record headers in the first stream were parsed. This does not provide a per-file listing. See `research/ios-27.3/mtree/mtree-report.md`.
