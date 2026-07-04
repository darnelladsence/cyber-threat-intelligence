# Cyber Metadata — ASN & Threat-Intelligence Datasets (MMDB)

Self-built, versioned IP-intelligence datasets in [MaxMind DB (MMDB)](https://maxmind.github.io/MaxMind-DB/)
format — the data layer behind [ipok.dev](https://ipok.dev)'s IP cleanliness / reputation
scoring. Built from public routing data (RouteViews) and curated threat feeds; every
record carries its `source`, every release ships a manifest with SHA-256 checksums,
and consecutive releases are diffable.

[English](#datasets) · [中文](#中文简介)

## Datasets

| File | Records | Contents |
|---|---|---|
| `cyber-metadata-asn.mmdb` | ~1.36 M prefixes | IP prefix → ASN ownership |
| `cyber-metadata-threat-ioc.mmdb` | ~28 K prefixes | IP prefix → threat-IOC reputation |
| `dataset_manifest.json` | — | release version, per-file rows + SHA-256 |

### Schema

**`cyber-metadata-asn.mmdb`** — longest-prefix match on IPv4/IPv6:

| Field | Type | Description |
|---|---|---|
| `asn` | string | Autonomous System Number, numeric string, e.g. `"13335"` |
| `as_name` | string | AS / operator name, e.g. `"CLOUDFLARENET"` |
| `country_code` | string | ISO 3166-1 alpha-2 registration country |
| `source` | string | data source id, e.g. `"routeviews"` |

**`cyber-metadata-threat-ioc.mmdb`** — prefixes with threat-intelligence hits:

| Field | Type | Description |
|---|---|---|
| `threat_type` | string | e.g. `"malware"`, `"botnet_c2"` |
| `confidence` | string | hit confidence `0–1` |
| `source` | string | feed id, e.g. `"abuse.ch"` |

Two sibling datasets (IP usage-type MMDB, de-identified WHOIS corpus) are used by
the scoring engine but are **not offered for download** — see the
[datasets page](https://ipok.dev/datasets) for what is public and why. Third-party
geolocation MMDBs (DB-IP, IPinfo) are distributed separately on that page under
their own CC licenses and are deliberately not part of this repository.

## Download

- **GitHub Releases** (this repo): versioned snapshots, one release per dataset build.
- **Always-latest URLs**:

```
https://ipok.dev/v1/datasets/current/files/cyber-metadata-asn.mmdb
https://ipok.dev/v1/datasets/current/files/cyber-metadata-threat-ioc.mmdb
https://ipok.dev/v1/datasets/current/files/dataset_manifest.json
```

Versioned releases, per-release diffs and a change timeline:
[ipok.dev/datasets](https://ipok.dev/datasets).

Verify integrity against the manifest:

```bash
python3 -c "import hashlib;print(hashlib.sha256(open('cyber-metadata-asn.mmdb','rb').read()).hexdigest())"
# compare with .files["cyber-metadata-asn.mmdb"].sha256 in dataset_manifest.json
```

## Quickstart

```bash
# libmaxminddb CLI
mmdblookup --file cyber-metadata-asn.mmdb --ip 1.1.1.1
```

```python
# pip install maxminddb
import maxminddb
with maxminddb.open_database("cyber-metadata-asn.mmdb") as r:
    print(r.get("1.1.1.1"))   # {'asn': '13335', 'as_name': 'CLOUDFLARENET', ...}
```

```go
// go get github.com/oschwald/maxminddb-golang
db, _ := maxminddb.Open("cyber-metadata-threat-ioc.mmdb")
defer db.Close()
var rec map[string]any
_ = db.Lookup(netip.MustParseAddr("203.0.113.7"), &rec)
```

No lookup infrastructure? The hosted API serves the same data:
`curl ipok.dev/8.8.8.8` — or use the [`cmeta` CLI](https://github.com/darnelladsence/cyber-meta-cli).

## Versioning

Releases are date-versioned (`2026.07.03`). Each GitHub Release mirrors one build
of the public files; the site keeps the recent release archive with diffs between
consecutive versions, so any verdict can be traced to the exact data version that
produced it.

## License & citation

Free for research and personal use; attribution to **Cyber Metadata** required.
No resale, redistribution of the raw files, or commercial re-licensing without
prior permission. Provided "as is". Full text: [LICENSE.md](LICENSE.md).

```bibtex
@misc{cybermetadata-datasets,
  author       = {Cyber Metadata},
  title        = {Cyber Metadata ASN and Threat-Intelligence Datasets},
  howpublished = {\url{https://ipok.dev/datasets}},
  note         = {Versioned MMDB releases},
  year         = {2026}
}
```

## 中文简介

自建、版本化的 IP 情报数据集（MMDB 格式），是 [ipok.dev](https://ipok.dev)
IP 纯净度评分的数据底座：

- **ASN 前缀库**（约 136 万前缀）：IP 前缀 → ASN 归属 / 运营商 / 注册国；
- **威胁情报 IOC 库**（约 2.8 万前缀）：IP 前缀 → 威胁类型 / 置信度 / 来源。

每个版本附带 manifest（行数 + SHA-256），版本间可
[在线对比变更](https://ipok.dev/datasets)。可免费用于研究与个人用途，使用时需注明来源
Cyber Metadata；未经许可不得转售或再分发原始文件。在线查询无需下载：
`curl ipok.dev/8.8.8.8`，或使用 [`cmeta` 命令行工具](https://github.com/darnelladsence/cyber-meta-cli)。
