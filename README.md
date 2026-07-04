# Cyber Metadata Datasets

Self-built, versioned IP-intelligence datasets behind [ipok.dev](https://ipok.dev) —
MMDB + CSV, one GitHub Release per build, SHA-256 manifest, diffable between versions.

## Datasets

| File | Records | Contents |
|---|---|---|
| `cyber-metadata-asn.mmdb` | ~1.36 M prefixes | IP prefix → ASN, operator name, registration country |
| `cyber-metadata-threat-ioc.mmdb` | ~28 K prefixes | IP prefix → threat type, confidence, source feed |
| `whois_public.csv.gz` | ~1.46 M domains | domain → dates / NS / registrar / registrant country / DNSSEC (de-identified) |
| `dataset_manifest.json` | — | release version, per-file rows + SHA-256 |

Field-level schema: [ipok.dev/datasets](https://ipok.dev/datasets). The IP usage-type
MMDB powers the scoring engine but is not offered for download.

`geo-v*` releases additionally mirror two **third-party** geolocation MMDBs
(DB-IP City Lite, CC BY 4.0 · IPinfo Lite, CC BY-SA 4.0) under their providers'
own licenses — see [THIRD_PARTY_LICENSES.md](THIRD_PARTY_LICENSES.md).

## Download

**[GitHub Releases](../../releases)**, or always-latest:

```
https://ipok.dev/v1/datasets/current/files/cyber-metadata-asn.mmdb
https://ipok.dev/v1/datasets/current/files/cyber-metadata-threat-ioc.mmdb
https://ipok.dev/v1/datasets/current/files/whois_public.csv.gz
https://ipok.dev/v1/datasets/current/files/dataset_manifest.json
```

Verify against the `sha256` entries in `dataset_manifest.json`. Per-release
diffs and the change timeline live at [ipok.dev/datasets](https://ipok.dev/datasets).

## Quickstart

```python
# pip install maxminddb
import maxminddb
with maxminddb.open_database("cyber-metadata-asn.mmdb") as r:
    print(r.get("1.1.1.1"))   # {'asn': '13335', 'as_name': 'CLOUDFLARENET', ...}
```

No downloads needed for one-off lookups — `curl ipok.dev/8.8.8.8`, or the
[`cmeta` CLI](https://github.com/darnelladsence/cyber-meta-cli).

## License & citation

Free for research and personal use, attribution to **Cyber Metadata** required;
no resale or redistribution of the raw files without permission. Full terms:
[LICENSE.md](LICENSE.md).

```bibtex
@misc{cybermetadata-datasets,
  author       = {Cyber Metadata},
  title        = {Cyber Metadata Datasets: ASN, Threat-IOC and WHOIS},
  howpublished = {\url{https://ipok.dev/datasets}},
  year         = {2026}
}
```

## 中文

ipok.dev 背后的自建 IP 情报数据集：ASN 前缀库（约 136 万前缀）、威胁情报 IOC 库
（约 2.8 万前缀）、WHOIS 域名注册库（公开脱敏版，约 146 万域名）。版本化发布、附
SHA-256 manifest、版本间可[在线对比](https://ipok.dev/datasets)。可免费用于研究与
个人用途，使用时注明来源 Cyber Metadata。在线查询无需下载：`curl ipok.dev/8.8.8.8`。
