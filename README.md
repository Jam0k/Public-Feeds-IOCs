# ThreatCluster — Public Feeds & IOCs

Free, no-auth threat intelligence feeds from [ThreatCluster](https://threatcluster.io).
Clustered and deduplicated from 20,000+ sources.

**TLP:CLEAR** — free to use, redistribute, and integrate. Attribution appreciated.

---

## Files in this repo

Snapshotted daily from the API. Use these if you want a pinned, versioned copy —
or `git log` to see when an indicator first appeared.

| File | Contents |
|---|---|
| [`feeds/domains.txt`](feeds/domains.txt) | Domains, one per line |
| [`feeds/ips.txt`](feeds/ips.txt) | IPv4 / IPv6 |
| [`feeds/hashes.txt`](feeds/hashes.txt) | MD5 / SHA-1 / SHA-256 |
| [`feeds/all.txt`](feeds/all.txt) | Domains + IPs combined |
| [`feeds/all.csv`](feeds/all.csv) | Full metadata (type, confidence, sources, reason) |
| [`feeds/all.json`](feeds/all.json) | Same, JSON |
| [`feeds/hashes.csv`](feeds/hashes.csv) | Hashes with metadata |
| [`feeds/stats.json`](feeds/stats.json) | Counts by type |

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
| [`/api/iocs/public/ips.txt`](https://threatcluster.io/api/iocs/public/ips.txt) | text | IPv4/IPv6 only |
| [`/api/iocs/public/hashes.txt`](https://threatcluster.io/api/iocs/public/hashes.txt) | text | MD5 / SHA-1 / SHA-256 |
| [`/api/iocs/public/hashes.csv`](https://threatcluster.io/api/iocs/public/hashes.csv) | CSV | Hashes with metadata |
| [`/api/iocs/public/stats`](https://threatcluster.io/api/iocs/public/stats) | JSON | Counts by type, window, freshness |

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

Per-cluster STIX bundles for SIEM/TIP ingestion:

```
GET /api/threats/{cluster_id}/stix
```

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

- **No authentication.** No API key, no rate limit for reasonable use.
- **Caching.** Feeds are cached server-side; polling more often than hourly
  gains you nothing.
- **Scope.** The public feed covers high-confidence network indicators from the
  last 30 days. Full corpus, all entity types and historical data are available
  via the [API](https://threatcluster.io/api/public/v1/docs).

## Links

- [Feed directory](https://threatcluster.io/feeds)
- [IOC browser](https://threatcluster.io/iocs)
- [API documentation](https://threatcluster.io/api/public/v1/docs)
- [Format matrix](https://threatcluster.io/formats)

---

## Current feed

<!--STATS-->
_Last updated: 2026-08-27 09:21 UTC_

**32 network indicators** · **320 hashes** · 30-day window

| Type | Count |
|---|---|
| md5 | 242 |
| sha256 | 64 |
| domain | 16 |
| ipv4 | 16 |
| sha1 | 14 |
<!--/STATS-->
