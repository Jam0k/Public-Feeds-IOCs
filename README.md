# ThreatCluster — Public Feeds & IOCs

Free, no-auth threat intelligence feeds from [ThreatCluster](https://threatcluster.io).
Clustered and deduplicated from 20,000+ sources.

**TLP:CLEAR** — free to use, redistribute, and integrate. Attribution appreciated.

---

## Files in this repo

Snapshotted daily from the API. Use these if you want a pinned, versioned copy —
or `git log` to see when an indicator first appeared.

**`feeds/`** — the block-ready set (last 30 days):

| File | Contents |
|---|---|
| [`feeds/domains.txt`](feeds/domains.txt) · [`.csv`](feeds/domains.csv) | Domains |
| [`feeds/ips.txt`](feeds/ips.txt) · [`.csv`](feeds/ips.csv) | IPv4 / IPv6 |
| [`feeds/hashes.txt`](feeds/hashes.txt) · [`.csv`](feeds/hashes.csv) | MD5 / SHA-1 / SHA-256 |
| [`feeds/crypto.txt`](feeds/crypto.txt) · [`.csv`](feeds/crypto.csv) | BTC / ETH / XMR wallets |
| [`feeds/all.txt`](feeds/all.txt) | Domains + IPs combined |
| [`feeds/all.csv`](feeds/all.csv) · [`.json`](feeds/all.json) | Full metadata |
| [`feeds/stats.json`](feeds/stats.json) | Counts by type |

**`feeds/full/`** — the whole archive since Nov 2025. Same filenames, and
deliberately a separate directory so a full list never lands in a blocklist by
accident. See [Full history](#full-history) before using it.

```bash
# pin to this repo instead of the live API
curl -s https://raw.githubusercontent.com/Jam0k/Public-Feeds-IOCs/main/feeds/domains.txt
```

The API below is the live source and always current; these files lag by up to a day.

---

## Quick start

```bash
# Block-ready domains + IPs (plain text, one per line)
curl https://threatcluster.io/api/iocs/public/feed.txt

# Same data with context (type, confidence, sources, reason)
curl https://threatcluster.io/api/iocs/public/feed.csv

# Feed health / counts
curl https://threatcluster.io/api/iocs/public/stats
```

---

## IOC feeds

High-confidence indicators from the last **30 days**.

| Endpoint | Format | Contents |
|---|---|---|
| [`/api/iocs/public/feed.txt`](https://threatcluster.io/api/iocs/public/feed.txt) | text | Domains + IPv4 + IPv6, one per line, `#` comments |
| [`/api/iocs/public/feed.csv`](https://threatcluster.io/api/iocs/public/feed.csv) | CSV | Full metadata incl. sources and reasoning |
| [`/api/iocs/public/feed.json`](https://threatcluster.io/api/iocs/public/feed.json) | JSON | Structured, same fields as CSV |
| [`/api/iocs/public/domains.txt`](https://threatcluster.io/api/iocs/public/domains.txt) | text | Domains only |
| [`/api/iocs/public/domains.csv`](https://threatcluster.io/api/iocs/public/domains.csv) | CSV | Domains with metadata |
| [`/api/iocs/public/ips.txt`](https://threatcluster.io/api/iocs/public/ips.txt) | text | IPv4/IPv6 only |
| [`/api/iocs/public/ips.csv`](https://threatcluster.io/api/iocs/public/ips.csv) | CSV | IPs with metadata |
| [`/api/iocs/public/hashes.txt`](https://threatcluster.io/api/iocs/public/hashes.txt) | text | MD5 / SHA-1 / SHA-256 |
| [`/api/iocs/public/hashes.csv`](https://threatcluster.io/api/iocs/public/hashes.csv) | CSV | Hashes with metadata |
| [`/api/iocs/public/crypto.txt`](https://threatcluster.io/api/iocs/public/crypto.txt) | text | Attacker wallets (BTC/ETH/XMR) |
| [`/api/iocs/public/crypto.csv`](https://threatcluster.io/api/iocs/public/crypto.csv) | CSV | Wallets with metadata |
| [`/api/iocs/public/stats`](https://threatcluster.io/api/iocs/public/stats) | JSON | Counts by type, window, freshness |

Every `.csv` above shares the same column set, so a parser written against one
works on all of them.

### Wallets

Attacker-controlled cryptocurrency addresses — ransom payments, exchange
thefts, scam proceeds. These are for chain analysis and payment screening, and
are deliberately **not** in `feed.txt`: a wallet address in a DNS blocklist is
meaningless. Every address is checksum-validated before it ships, so a mistyped
or truncated one never reaches the feed.

### Lookup a single indicator

```bash
curl "https://threatcluster.io/api/iocs/lookup?value=example.com"
```

### CSV fields

```
type,value,confidence,first_seen,last_seen,source_count,sources,reason
```

`reason` is the analyst-facing justification — why this indicator is considered
attacker-controlled. Useful for triage and for filing disputes.

```csv
domain,drive.google.verify-drive.com,high,2026-08-21T00:32:07Z,2026-08-21T00:32:07Z,1,cloud.google.com:2026-08-21,Identified as a malware distribution domain.
```

---

## Full history

Everything above covers the last 30 days — the **block-ready** set: recent,
corroborated, safe to point a firewall at. The `full/` endpoints are the whole
archive back to November 2025, with the date each indicator was first seen.

> **Read this before you use it.** This is a hunt and enrichment dataset, not a
> blocklist. Infrastructure ages: a C2 address from last winter may since have
> been reallocated to somebody uninvolved, and blocking it would hit them
> rather than the actor. Search your logs against it, enrich with it, pivot on
> it — don't sinkhole it.

| Endpoint | Format |
|---|---|
| [`/api/iocs/public/full/domains.txt`](https://threatcluster.io/api/iocs/public/full/domains.txt) · [`.csv`](https://threatcluster.io/api/iocs/public/full/domains.csv) | Every domain |
| [`/api/iocs/public/full/ips.txt`](https://threatcluster.io/api/iocs/public/full/ips.txt) · [`.csv`](https://threatcluster.io/api/iocs/public/full/ips.csv) | Every IP |
| [`/api/iocs/public/full/hashes.txt`](https://threatcluster.io/api/iocs/public/full/hashes.txt) · [`.csv`](https://threatcluster.io/api/iocs/public/full/hashes.csv) | Every hash |
| [`/api/iocs/public/full/crypto.txt`](https://threatcluster.io/api/iocs/public/full/crypto.txt) · [`.csv`](https://threatcluster.io/api/iocs/public/full/crypto.csv) | Every wallet |
| [`/api/iocs/public/full/feed.csv`](https://threatcluster.io/api/iocs/public/full/feed.csv) · [`.json`](https://threatcluster.io/api/iocs/public/full/feed.json) | All types, full metadata |
| [`/api/iocs/public/full/stats`](https://threatcluster.io/api/iocs/public/full/stats) | Counts and date coverage |

**Hashes and wallets age best.** A file hash names one exact artefact forever,
and a wallet is bound to whoever holds the key — neither can be reassigned to
an innocent party the way a domain or an address can. If you only take one full
list, take those.

Corroboration filter — indicators reported by two or more independent
publishers:

```bash
curl "https://threatcluster.io/api/iocs/public/full/feed.json?min_sources=2"
```

---

## MISP

Native MISP feed — add as a URL feed in your MISP instance.

| Endpoint | Purpose |
|---|---|
| [`/misp/manifest.json`](https://threatcluster.io/misp/manifest.json) | Feed manifest (event index) |
| `/misp/{event_uuid}.json` | Individual MISP event |
| [`/misp/hashes.csv`](https://threatcluster.io/misp/hashes.csv) | Hash-only CSV |

**Setup:** MISP → Sync Actions → Feeds → Add Feed
- Input Source: `Network`
- URL: `https://threatcluster.io/misp`
- Source Format: `MISP Feed`

Events are tagged `tlp:clear`, `source:ThreatCluster`, and by threat level.

---

## STIX 2.1

Per-cluster STIX bundles (report, indicators, inferred relationships,
TLP-marked) for SIEM/TIP ingestion. They need a key; a free one is minted on
every account and covers 100 credits a day, and a bundle costs 3.

```bash
curl -H "X-API-Key: $TC_KEY" \
  "https://threatcluster.io/api/public/v1/threats/{cluster_id}/stix"
```

Every cluster page on the site has an **API** button that shows this request
for the record you are looking at. Quick start: https://threatcluster.io/api

---

## RSS

| Feed | URL |
|---|---|
| Threat clusters | [`/feed.xml`](https://threatcluster.io/feed.xml) |
| Vulnerabilities | [`/vulnerabilities/feed.xml`](https://threatcluster.io/vulnerabilities/feed.xml) |
| Exploits | [`/exploits/feed.xml`](https://threatcluster.io/exploits/feed.xml) |
| Dark web victims | [`/dark-web/feed.xml`](https://threatcluster.io/dark-web/feed.xml) |

Import all at once: [`/feeds.opml`](https://threatcluster.io/feeds.opml)

---

## Also published on

Indicators are shared to the community platforms below, so you can consume them
wherever you already work:

| Platform | Profile |
|---|---|
| AlienVault OTX | [projectargus](https://otx.alienvault.com/user/projectargus/pulses) |
| VirusTotal | [ThreatCluster.io](https://www.virustotal.com/gui/user/ThreatCluster.io) |
| ThreatFox (abuse.ch) | [ThreatCluster](https://threatfox.abuse.ch/user/82055/) |

---

## Reporting an indicator

Found something that shouldn't be listed? Email **hello@threatcluster.io** with
the indicator and we'll review and remove it.

---

## Usage notes

- **No authentication.** No API key on any feed on this page. Rate limit is
  60 requests a minute per IP, which is far more than a feed needs.
- **Caching.** Feeds are cached server-side; polling more often than hourly
  gains you nothing.
- **Scope.** The default feeds cover high-confidence indicators from the last
  30 days. [Full history](#full-history) goes back to November 2025. Per-cluster
  IOCs, entity and CVE records, search and the dark-web records are in the
  [API](https://threatcluster.io/api): a free key on every account, 100 credits
  a day, no card.
- **What gets excluded.** Indicators are dropped at export if they resolve to
  government, academic or e-government domains, freemail providers, known
  abused third-party services (paste sites, tunnelling, mining pools), bare
  network ranges, or placeholder hostnames from a redacted advisory. Most
  candidate indicators do not make it — that is deliberate.

## Links

- [Feed directory](https://threatcluster.io/feeds)
- [IOC browser](https://threatcluster.io/iocs)
- [API quick start](https://threatcluster.io/api) · [reference](https://threatcluster.io/api/public/v1/docs) · [client + examples](https://github.com/Jam0k/Cyber-Threat-Intelligence-API)
- [Integration guides](https://threatcluster.io/integrations) (Splunk, Sentinel, Elastic, Claude, OpenAI, Cursor, the terminal)
- [Format matrix](https://threatcluster.io/formats)
- Ransomware leak-site data: [Ransomware-Intel](https://github.com/Jam0k/Ransomware-Intel)

---

## Current feed

<!--STATS-->
_Last updated: 2026-09-04 06:38 UTC_

**138 network indicators** · **665 hashes** · **54 wallets** · 30-day window

| Type | Last 30 days | All time |
|---|---|---|
| sha256 | 378 | 1144 |
| md5 | 271 | 966 |
| domain | 92 | 362 |
| sha1 | 16 | 200 |
| ipv4 | 46 | 197 |
| eth | 52 | 94 |
| xmr | 1 | 3 |
| btc | 1 | 3 |
| ipv6 | 0 | 1 |
<!--/STATS-->
