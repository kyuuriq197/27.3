# Cryptex report - iOS 27.3 metadata and prior listing

## Target 27.3

The BuildManifest assigns six components to the `Cryptex1` group. The two restore identities reference the same component paths; statuses below are direct SHA-384 comparisons against ZIP-entry bytes, not signature or provenance validation.

| Component | Archive entry | Bytes | Direct archive digest check |
|---|---|---:|---|
| `Cryptex1,AppOS` | `141-93238-032.dmg` | 18874368 | Match |
| `Cryptex1,AppTrustCache` | `Firmware/141-93238-032.dmg.trustcache` | 863 | Match |
| `Cryptex1,AppVolume` | `Firmware/141-93238-032.dmg.root_hash` | 229 | Match |
| `Cryptex1,SystemOS` | `141-94047-030.dmg.aea` | 2415919104 | Mismatch |
| `Cryptex1,SystemTrustCache` | `Firmware/141-94047-030.dmg.aea.trustcache` | 3191 | Match |
| `Cryptex1,SystemVolume` | `Firmware/141-94047-030.dmg.aea.root_hash` | 229 | Match |

The Cryptex SystemOS AEA container is 2,415,919,104 bytes (SHA-256 `c3bfebcb86b324748160b33559d8a5efc40955abc00ad842b2116409f82f83e1`). Its metadata parser succeeded; only WKMS field names were persisted. No key lookup, decryption, or filesystem mount was attempted.

The associated SystemTrustCache contains 131 parsed entries, with zero duplicate CDHashes. This is signing-metadata inventory only; it does not enumerate binaries inside the encrypted Cryptex.

The 229-byte SystemVolume sidecar is an unencrypted IM4P wrapper of type `cssy` with a 208-byte payload. Only wrapper metadata and the sidecar file hash were recorded; inner root-hash semantics were not decoded.

## Previous archived 27.2 listing

The saved `work/cryptex1_systemos_listing.txt.gz` was streamed and structurally checked: 3,085 rows, 893 directories, 2,190 files, 2 symlinks, and no malformed rows. This validates the archived text format and counts, not the original extraction or the contents of the exact old IPSW.

Most frequent file extensions in that listing:

- `.strings`: 905
- `.stringsdict`: 735
- `.js`: 173
- `[no extension]`: 74
- `.plist`: 56
- `.md`: 24
- `.svg`: 24
- `.dylib`: 23

The archived listing names 85 arm64e dyld-cache files totaling 7,400,102,867 bytes. This is path and size accounting only:
- DyldData: 3 files, 618,840,064 bytes
- DyldLinkedit: 1 file, 694,730,752 bytes
- DyldReadOnly: 3 files, 200,196,096 bytes
- NumberedSegment: 72 files, 4,597,874,688 bytes
- Other: 4 files, 37,772,243 bytes
- Symbols: 2 files, 1,250,689,024 bytes

It also contains 39 framework directory entries and 2,059 files under framework paths (90,646,249 bytes total). This does not establish that each binary was parsed or independently verified.

That listing includes dyld-cache files, but no 27.3 dyld cache was extracted. Its contents cannot be compared to 27.3 because the current Cryptex image remains encrypted and the exact previous IPSW was not available.

## Limits

- No AEA keys were obtained or used; no decryption or APFS parsing was performed.
- BuildManifest digest mismatches for the AEA ZIP entry remain unresolved. They are reported as direct-hash comparison results, not as evidence of corruption or tampering.
- The 27.2 listing is archived evidence, not a reproducible 27.3 extraction or a full binary comparison.

Machine-readable summary: `research/ios-27.3/cryptex/cryptex_summary.json`.
