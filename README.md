# iPhone14,5 iOS 27.3 research

## Status

The local IPSW metadata and ZIP inventory are verified. Kernel and kext metadata, restore-ramdisk IM4P headers, AEA field names, Cryptex BuildManifest roles and sidecar metadata, and a read-only AppOS APFS file listing have been analyzed. The AppOS tree walk was cross-checked with an independent reader; AEA Cryptex contents and exact-file Apple provenance remain unverified. No vulnerability assessment was performed.

## Reports

- [Firmware inventory](firmware/inventory-report.md)
- [Firmware plist metadata](firmware/firmware-plist-report.md)
- [BuildManifest digest cross-check](manifest/buildmanifest-report.md)
- [AEA metadata report](aea/aea-report.md)
- [Kernel report](kernel/kernel-report.md)
- [IM4P metadata inventory](img4/img4-report.md)
- [Trustcache metadata](trustcache/trustcache-report.md)
- [dyld report](dyld/dyld-report.md)
- [Kext inventory](kext/kext-inventory-report.md)
- [Filesystem/APFS report](apfs/filesystem-report.md)
- [SystemOS mtree sidecar report](mtree/mtree-report.md)
- [Cryptex report](cryptex/cryptex-report.md)
- [Restore ramdisk report](ramdisk/ramdisk-report.md)
- [Restore ramdisk APFS inventory](ramdisk/apfs-inventory-report.md)
- [Restore ramdisk APFS inventory](ramdisk/apfs-inventory-report.md)
- [Security scope report](security/security-report.md)
- [Differential report](differential/differential-report.md)
- [Interim final summary](reports/final-summary.md)
- [Windows toolchain record](reports/toolchain.md)
- [Task and phase status](TASKS.md)

## Machine outputs

- `firmware/ipsw_metadata.json`
- `firmware/ipsw_inventory.csv`
- `firmware/ipsw_sha256.txt`
- `firmware/ipsw_parser_info.json`
- `firmware/firmware_plist_inventory.json`
- `manifest/buildmanifest_digest_check.csv`
- `manifest/buildmanifest_digest_summary.json`
- `kernel/kernel_version.json`
- `kernel/kernel_analysis_summary.json`
- `kernel/kernel_macho_header_loads.txt`
- `kext/kext_inventory_27.3.json`
- `ramdisk/ramdisk_metadata_summary.json`
- `ramdisk/apfs_inventory_summary.json`
- `ramdisk/141-93054-032.dmg.im4p-info.json`
- `ramdisk/141-92811-032.dmg.im4p-info.json`
- `aea/aea_metadata_summary.json`
- `apfs/appos_header_summary.json`
- `apfs/appos_superblock_summary.json`
- `apfs/appos_integrity_meta_summary.json`
- `apfs/ferrosys_appos_inspection.json`
- `apfs/hadris_appos_validation.json`
- `apfs/root_hash_sidecar_inventory.json`
- `mtree/mtree_metadata_summary.json`
- `mtree/mtree_lzfse_summary.json`
- `img4/im4p_inventory_summary.json`
- `trustcache/trustcache_summary.json`
- `trustcache/trustcache_overlap_summary.json`
- `trustcache/trustcache-overlap-report.md`
- `cryptex/cryptex_summary.json`
- `differential/kext_inventory_diff.csv`
- `differential/kext_inventory_diff_summary.json`
- `logs/kernel-analysis-run.txt`
- `logs/ramdisk-metadata-run.txt`
- `logs/aea-metadata-run.txt`
- `logs/appos-header-run.txt`
- `logs/appos-rust-walk.txt`
- `logs/ferrosys-appos-inspection.txt`
- `logs/dyld-extraction-attempt.txt`
- `logs/im4p-inventory-run.txt`
- `logs/trustcache-summary-run.txt`
- `logs/root-hash-inventory-run.txt`
- `logs/mtree-metadata-run.txt`
- `logs/firmware-plist-inventory-run.txt`
- `differential/ipsw_file_inventory_diff.csv`
- `differential/ipsw_file_inventory_diff_summary.json`
- `reports/windows_environment.json`

The IPSW itself and extracted firmware images are not copied into this tree.
