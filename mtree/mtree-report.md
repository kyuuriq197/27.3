# SystemOS canonical metadata decode - iOS 27.3

## Verified source and decoding

The unencrypted `msys` sidecar payload matches the recorded byte length and SHA-256. A pinned pure-Rust LZFSE decoder (`lzfoo 0.2.0`) decoded the framed LZFSE streams after the 28-byte `pbze` wrapper header. Each stream terminates with `bvx$`; 16 bytes between consecutive streams were skipped as framing bytes and not interpreted.

- Input payload: 38547937 bytes, SHA-256 `6b0193b1ed7b609700fab7e0a0f1347c371dbd4b6d05f6f62e9862568b39a79a`
- LZFSE streams: 37; decoded bytes total: 153249671
- Decoded stream sizes:
- 2254727: 1 streams
- 4194304: 36 streams

## First Apple Archive index chunk

The first decoded stream is 4194304 bytes and begins with `AA01`. Exactly 2 bounded record headers were parsed before trailing non-record data.
- `D`: 1 records
- `F`: 1 records
The record headers occupy 157 bytes; the remaining 4194147 bytes in this first stream were not interpreted.

Paths, hashes, record values, and file payloads were not saved. This confirms the current build has an Apple Archive index followed by additional opaque metadata; it does not establish a per-file listing.

## Limits

- The 27.3 AEA Cryptex SystemOS image remains encrypted; it was not decrypted or mounted.
- The remaining LZFSE streams and trailing hash-tree data were not semantically parsed.
- This metadata decode does not validate Apple provenance or establish a security impact.

Machine-readable summary: `research/ios-27.3/mtree/mtree_lzfse_summary.json`.
