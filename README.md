# Signature Feeds

Daily Holloman fingerprint signature feeds for YARA, ClamAV, and Zeek.

Generated from Redis upload pipeline — 30–77K files/day, 88–100% tagged
with  family labels and  categories.

## Feeds

| Feed | Format | Usage |
|------|--------|-------|
| `yara/feed-YYYYMMDD.yar` | YARA rules | `yara-g -r feed-20260630.yar target.exe` |
| `clamav/feed-YYYYMMDD.hlo` | ClamAV `.hlo` signatures | `clamscan -d feed-20260630.hlo malware.exe` |
| `zeek/feed-YYYYMMDD.intel` | Zeek Intel feed | `redef Intel::read_files += {"feed-20260630.intel"};` |

## Pipeline

```
Redis tg-uploads → SHA256 → SHA1 → h128 fingerprint → @cluster_id tags
                                                           ├── vt (preferred)
                                                           └── tg:cat (fallback)
```

## Papers

| Document | Repository | Description |
|----------|------------|-------------|
| [ClamAV Integration](https://github.com/wessorh/clamav-holloman3/blob/main/holloman-integration.pdf) | clamav-holloman3 | Holloman fingerprint detection in ClamAV: architecture, benchmarks (14.2 ms/file), test suite results |
| [Zeek Integration](https://github.com/wessorh/clamav-holloman3/blob/main/holloman-integration.pdf) | clamav-holloman3 | Combined ClamAV + Zeek report: C++ wrapper, OpaqueVal, Intel framework |
| [Cluster Analysis](https://github.com/wessorh/yara-be/blob/main/report.tex) | yara-be | Similarity hashing comparison: ssdeep, TLSH, SimHash, MinHash, Holloman, fpHash on 5,000 malware samples |
| [Advanced Integration Plan](https://github.com/wessorh/clamav-holloman3/blob/main/ADVANCED-PLAN.md) | clamav-holloman3 | CVD packaging, per-order partitioning, bytecode runtime integration |
| [ClamAV Benchmark](https://github.com/wessorh/clamav-holloman3/blob/main/BENCHMARK-REPORT.md) | clamav-holloman3 | 1,000 PE files benchmark: 14.2 ms/file, 121.4 MB/s |

## Tools

| Tool | Repository | Purpose |
|------|------------|---------|
| `source2yara.py` | [tools](https://github.com/wessorh/tools) | Generate YARA rules from ClickHouse source |
| `yar2hlo.sh` | [tools](https://github.com/wessorh/tools) | Convert YARA rules to ClamAV `.hlo` format |
| `hash2yara.py` | [tools](https://github.com/wessorh/tools) | SHA256 hashes → YARA rules |
| `signature-feeds.py` | [tools](https://github.com/wessorh/tools) | Redis uploads → YARA/ClamAV/Zeek signature files |

## Repositories

| Repo | URL | Description |
|------|-----|-------------|
| signature-feeds | [github.com/wessorh/signature-feeds](https://github.com/wessorh/signature-feeds) | Daily signature feeds |
| clamav-holloman3 | [github.com/wessorh/clamav-holloman3](https://github.com/wessorh/clamav-holloman3) | ClamAV + Zeek integration |
| yara-be | [github.com/wessorh/yara-be](https://github.com/wessorh/yara-be) | YARA engine, cluster analysis, MIH search |
| tools | [github.com/wessorh/tools](https://github.com/wessorh/tools) | Signature generation toolchain |
