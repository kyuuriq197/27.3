# iOS 27.3 research progress

Updated: 2026-11-07

## Completed

- [x] Audited the downloaded repository snapshot: README, `.gitignore`, all tracked-looking files under `work/`, and the existing parser/report artifacts.
- [x] Confirmed the selected project folder contains no `.git` metadata. No branch or commit can be created from this folder without first obtaining a real Git checkout.
- [x] Confirmed the supplied IPSW exists at `C:\Users\Filimoshki\Downloads\iPhone14,5_27.3_24C5037i_Restore.ipsw` and is 11,483,532,526 bytes.
- [x] Read local IPSW plist metadata: ProductType `iPhone14,5`, ProductVersion `27.3`, ProductBuildVersion `24C5037i`, BuildTrain `RaveCSeed`, device class `d17ap`.
- [x] Confirmed `BuildManifest.plist` names restore ramdisks `141-93054-032.dmg` and `141-92811-032.dmg`.
- [x] Added Windows PowerShell scripts for environment inventory, IPSW ZIP/plist inventory and hashing, and a cautious comparison with the saved 27.2 component inventory.
- [x] Added Windows setup, execution, and methodology documentation.
- [x] Recorded Windows 11 Pro x64, PowerShell 5.1, 31.91 GiB RAM, 12 logical CPUs, and 305.95 GiB free on the project drive; no software installed.
- [x] Ran the environment probe and full IPSW inventory; validated 113 entries, build identity, aggregate sizes, and the archive SHA-256.
- [x] Compared all 96 prior inventory records against 113 current entries; documented added/removed names, size changes, and truncated-hash limitations.
- [x] Stream-counted the saved 27.2 system and Cryptex listings; verified their reported record totals.
- [x] Stream-counted the saved 27.2 kernel and ramdisk strings listings; verified 116,614 and 217,451 lines.
- [x] Saved PowerShell transcripts for the IPSW inventory, kernel, ramdisk, AEA, AppOS, IM4P, trustcache, firmware-plist, and root-hash stages.
- [x] Added separate status/evidence reports for firmware, kernel, kexts, APFS, Cryptex, ramdisk, security scope, and differential results.
- [x] Cross-checked selected BuildManifest SHA-384 digests: kernel, both ramdisks, and AppOS matched; the two AEA image digests did not and are explicitly unresolved.
- [x] Automated the BuildManifest digest cross-check across 154 component references and documented 124 direct whole-entry matches plus 30 role-dependent mismatches.
- [x] Added a reproducible manifest-digest checker, machine-readable results, and a report that avoids treating mismatches as authenticity or corruption findings.
- [x] Installed the pinned Windows x64 IPSW parser into the ignored project-local tools folder after verifying the GitHub release archive digest.
- [x] Extracted and parsed only the 27.3 kernel cache; validated Mach-O type/architecture/UUID/load-command counts and recorded the XNU version.
- [x] Parsed 269 unique 27.3 kext bundle IDs and paths; compared paths/version strings against the saved 27.2 list.
- [x] Added an idempotent Windows kernel-metadata runner and captured its transcript.
- [x] Parsed all 16 Windows PowerShell scripts for syntax errors; reran the kext differential and validated the 269/269 path counts and status totals.
- [x] Regenerated the research SHA-256 manifest and re-verified the listed portable artifact sizes and hashes.
- [x] Inspected IPSW parser help for AEA, IMG4, disk, and mount capabilities without attempting key lookup, decryption, or mounting.
- [x] Verified Apple's Developer Releases listing for iOS 27.3 beta build `24C5037i` (dated November 3, 2026); the listing corroborates the public build label, not the authenticity of this local IPSW.
- [x] Extracted both restore ramdisk IM4P entries, verified their archive hashes, parsed the `rdsk` headers, and extracted unencrypted payloads with the pinned Windows tool.
- [x] Reran the ramdisk stage successfully with existing outputs to verify its idempotent path.
- [x] Updated the artifact hash generator to exclude ignored extracted `data\` binaries and regenerated the documentation/results manifest.
- [x] Extracted both AEA containers, matched their sizes and SHA-256 values to the IPSW inventory, and ran the metadata parser while persisting only field names.
- [x] Extracted and hash-verified AppOS; recorded `NXSB` at offset 32 and the known limitation that the available DMG parser rejects this image.
- [x] Added a reproducible AppOS header check and recorded the DMG parser rejection without claiming APFS structure or file contents.
- [x] Attempted arm64e dyld extraction with the pinned Windows tool; it explicitly reported Windows unsupported and produced no cache. Recorded the blocker without treating the stage as complete.
- [x] Parsed IM4P header metadata for all 36 inventory-listed payloads after verifying extracted bytes; 36 parsed, 0 parser failures, no payload decryption.
- [x] Parsed five trustcaches and verified their parser-reported counts against 4,754 parsed records; no duplicate hashes were observed within the individual caches.
- [x] Extracted, size/hash-verified, and decoded all 22 inventory-listed firmware plists; portable reports retain field names rather than metadata values.
- [x] Verified the sizes and SHA-256 values of all three root-hash sidecars and recorded their literal marker strings without semantic claims.
- [x] Parsed the three root-hash sidecar IM4P headers as type `casy`/`isys`/`cssy`, version 0, unencrypted, 208-byte payloads; did not decode payload semantics.
- [x] Inspected the nested Veridian firmware-map schema from the locally decoded plist, recording only field names and JSON types/counts.
- [x] Cross-checked iOS version, build, product type, and board ID between the PowerShell plist inventory and `ipsw info --json`; all four agree.
- [x] Verified and unwrapped the SystemOS `msys` metadata IM4P sidecar; confirmed payload size, SHA-256, `pbze` signature, and literal `bvx2` marker offset without decompressing it.
- [x] Generated a repeatable Cryptex summary from the 27.3 BuildManifest, AEA field-name summary, root-hash wrapper metadata, trustcache counts, and the archived 27.2 Cryptex listing.
- [x] Validated all 3,085 archived 27.2 listing rows: 893 directories, 2,190 files, 2 symlinks, and no malformed rows; summarized common file extensions and largest files without claiming 27.3 contents.
- [x] Summarized the archived 27.2 listing's arm64e dyld-cache filename groups, bytes, and framework path totals; no dyld or framework binaries were parsed.
- [x] Confirmed six `Cryptex1` BuildManifest roles are represented in both restore identities; five direct SHA-384 ZIP-entry checks match and the encrypted `SystemOS` entry remains an unresolved mismatch.
- [x] Parsed all 18 Windows PowerShell scripts for syntax errors; regenerated the Cryptex and mtree reports and machine-readable summaries.
- [x] Installed the pinned pure-Rust `lzfoo` 0.2.0 decoder and decoded all 37 LZFSE streams from the unencrypted 27.3 `msys` sidecar; verified all streams end at the payload boundary.
- [x] Parsed the first decoded stream's Apple Archive record headers: two records (`D` and `F`), with paths, hashes, and values omitted; left the other stream contents and trailing hash material uninterpreted.
- [x] Detected the AppOS image as APFS with pinned `ferrosys` 0.7.0; its quick inspection refused the sealed-volume feature (`0x20`), then format-aware and independent Rust readers successfully traversed the AppOS tree.
- [x] Independently verified the first AppOS APFS superblock's 4 KiB geometry, object identifiers, and Fletcher64 checksum; this validates block zero only.
- [x] Parsed the sealed-volume integrity-metadata object mapped by OID 1172; its Fletcher checksum is valid and it reports SHA-256, root-hash offset 128, and `broken_xid=0`. The Merkle root itself was not validated.
- [x] Walked the headerless, hashed AppOS FS-tree: 128 nodes, 6,629 records, 127/127 child hashes valid, and 1,365 reachable paths with no traversal errors.
- [x] Independently walked AppOS with pinned `hadris-apfs 3.0.0-rc.3`; its sorted listing matched all 1,365 paths and kinds, with no known nonzero-size mismatches.
- [x] Parsed both unencrypted restore ramdisk payloads as APFS; the declared file-kind counts match, and `hadris-apfs` independently agrees on reachable paths and kinds. File-size fields differ between readers and are explicitly unresolved.
- [x] Checked an independent reader candidate: upstream `apfs-fuse` documents read-only access and sealed-volume support, but is Linux-only with no packaged releases; WSL is absent, so it was not built or run.
- [x] Tried the approved WSL setup path; Windows requires administrator elevation, which is not available in this session. No system changes or reboot occurred.
- [x] Added and ran a reproducible script comparing all five 27.3 trustcaches by code-signing hash; saved only aggregate and pairwise overlap counts, not hash values.

## Current status

- [x] Checked whether the exact 27.2 seed build named in the archived inventory (`24B5099f`) is available locally or discoverable through public sources. No exact file was found in Downloads/Desktop; the IPSW metadata API has no matching build, and Apple's release page has no exact-ID match. The API offered 27.2 build `24B119`, which is a different build and was not substituted.
- [x] Keep the existing 27.2 comparison explicitly limited to archived names, sizes, truncated SHA-256 prefixes, and separately recorded metadata until the exact original seed IPSW or full per-entry hashes are available.

## Remaining

- [ ] Obtain an Apple-hosted checksum or verify an Apple signature chain for this exact IPSW and device-specific BuildManifest.
- [x] Find and validate read-only Windows-native Rust parsers for both restore ramdisks and the AppOS APFS structures; the sealed AppOS FS-tree was walked manually and independently cross-checked.
- [ ] Determine whether the encrypted AEA images can be analyzed further through supported offline metadata only; do not obtain keys or decrypt.
- [ ] Find a documented Windows-native dyld parser; the pinned IPSW tool states dyld extraction is unsupported on Windows.
- [x] Find and validate Windows-native LZFSE decompression for the unencrypted mtree payload; only bounded Apple Archive header types are reported, not file paths or hashes.
- [x] Establish a reproducible Windows-native APFS analysis path for the unencrypted AppOS image; encrypted AEA Cryptex filesystems remain outside scope, and no AEA decryption was attempted.
- [ ] Explain the unmapped AppOS OID 1173; OID 1172 is now identified as checksum-valid sealed-volume integrity metadata.
- [ ] Obtain the exact original 27.2 build (`24B5099f`) from a verifiable source or user-provided path before performing a binary-level comparison; do not substitute another build.
- [ ] Git branch, incremental commits, and publication are blocked because the supplied folder is not a Git checkout; do not initialize or push this archive snapshot as if it had repository history.

## Errors and resolutions

- `git status` reports “not a git repository”: confirmed the folder has no `.git` directory. Resolution: preserve the snapshot and avoid fabricating history; document Git operations as blocked.
- `python` resolves to the WindowsApps alias, which did not provide a usable interpreter version. Resolution: current inventory tooling is PowerShell/.NET and requires no Python package installation.
- The public IPSW metadata endpoint did not return a record for `iPhone14,5 / 27.3 / 24C5037i`. Resolution: verified the build label against Apple's Developer Releases listing, while keeping exact-file/device-specific IPSW provenance unresolved.
- The IPSW metadata API did not match the archived 27.2 build `24B5099f`; it listed the different 27.2 build `24B119`. Resolution: did not substitute another build, and kept the old/new comparison limited to the saved inventory.
- A local search of Downloads/Desktop found no copy of the exact `24B5099f` IPSW, and Apple's release page did not return that build identifier. Resolution: stop before downloading a different build and request the exact source or local path.
- Default PowerShell invocation was blocked by the local execution policy. Resolution: use `powershell.exe -NoProfile -ExecutionPolicy Bypass` for the process only; no policy was changed.
- Initial environment/compare script runs exposed parameter-default and strict-mode optional-capture issues. Resolution: resolve defaults inside the script body and guard optional captures; reran both scripts successfully.
- Nested PowerShell command-line quoting initially altered a transcript invocation. Resolution: reran the inventory with a correctly encoded child-process command; the saved transcript contains the successful build, entry-count, and hash output.
- Direct SHA-384 over the two large AEA ZIP entry streams did not match their BuildManifest digest values; selected non-AEA component digests did match. Resolution: recorded the mismatch and actual hashes without attributing the cause or treating it as corruption.
- Full raw-entry digest cross-check found additional mismatches in restore-specific roles. Resolution: documented that `Digest` is not universally a whole-ZIP-entry hash and avoided treating these comparisons as authenticity verdicts.
- Initial kext comparison attempts exposed PowerShell 5 JSON-array enumeration and strict-mode pitfalls. Resolution: assign `ConvertFrom-Json` array output directly, use explicit maps for optional metadata, and validate the final 269-row path comparison.
- Found that the broad root `.gitignore` pattern `/t*` also matched `/tools`, shadowing the intended Windows tooling exceptions. Resolution: removed the redundant wildcard; `/t_decompress.img` remains explicitly ignored.
- The pinned tool's `disk dmg` command rejected the ramdisk wrapper as a DMG. Resolution: parsed the wrapper with its IM4P subcommand, verified `rdsk` / `encrypted=false`, extracted payload bytes without keys, then used two read-only APFS implementations to inventory the extracted payloads.
- The artifact hash generator initially included local-only files below ignored `research/ios-27.3/data/`. Resolution: exclude the generated data directory so the reproducibility manifest contains only portable project evidence and reports.
- The AEA metadata command writes some output on native stderr, which PowerShell 5 treated as a terminating error under `Stop`. Resolution: temporarily capture parser output in memory and persist only metadata field names; both AEA entries then parsed successfully without saving field values.
- The pinned DMG parser rejected the AppOS component, and `ferrosys` refused its sealed feature. Resolution: a format-aware APFS walker handled headerless hashed nodes; an independent reader matched the resulting path/kind inventory. The initial parser refusals are tool limitations, not evidence of image damage.
- The pinned `ipsw extract --dyld` command explicitly returned `dyld extraction is not supported on Windows`. Resolution: captured the full output and exit code; no cache or framework analysis is claimed.
- The first IM4P inventory attempt keyed components by basename and stopped at a nested `Firmware/` path. Resolution: compare each extracted relative path to the ZIP member path; rerun parsed and hash-verified all 36 entries successfully.
- Initial trustcache parser output was too large for a portable report. Resolution: keep full JSON only in ignored local data and write a compact summary containing component paths, versions, UUIDs, counts, and hash types, without individual code hashes.
- Firmware plist parsing is reproducible with the pinned Windows tool; decoded plist values may contain digest data, so only top-level field names are written to portable results and decoded JSON stays in ignored local data.
- Root-hash sidecars were small enough to hash and inspect directly; recorded literal markers only and did not interpret the DER-like structure or claim APFS validation.
- The mtree sidecar parser unwraps the `msys` IM4P; its framed LZFSE streams were decoded and two bounded Apple Archive record headers were parsed. Resolution: record the limited result and leave paths, values, remaining streams, and filesystem semantics uninterpreted.

## Next actions

1. Determine whether an Apple-hosted checksum or verifiable signature chain can corroborate the exact local IPSW.
2. If administrator access becomes available, provision a Linux environment and test the upstream read-only `apfs-fuse` reader against the hash-verified AppOS image; independently validate any resulting listing before claiming a filesystem inventory.
4. Investigate Windows-native dyld parsing only if a documented implementation is available; the pinned IPSW tool does not support it on Windows.
5. Keep AEA volumes encrypted; do not obtain keys or decrypt.
6. Obtain the exact `24B5099f` IPSW from a verifiable source or user-provided path; only then attempt the full binary comparison.
