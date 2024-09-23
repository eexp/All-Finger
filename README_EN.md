# Web Fingerprint All-in-One

![Allfinger Logo](https://raw.githubusercontent.com/eexp/pic/main/202408201730719.png)

[中文版](README.md)

## Introduction

**Allfinger** is a powerful tool for identifying and recording web fingerprints, featuring a database of over 60,000 fingerprints for fast and flexible scanning.

## Key Features

- 🚀 **Extensive Fingerprint Database**: Over 60,000 fingerprints for comprehensive coverage.
- ⚡ **Fast Scanning**: Efficient scanning speed to save time.
- 📂 **Flexible Export Options**: Supports multiple export formats to meet diverse needs.

## Quick Start

Get started with Allfinger using the following commands:

```bash
allfinger -u http://target.com 
# Scan a single target

allfinger -l /targets.txt
# Scan multiple targets from a file

allfinger -u http://target.com -t 200 
# Set thread count to 200

allfinger -u http://target.com -t 200 -o tg.xlsx
# Supports export in json, xlsx, db formats (currently db supports MySQL only, requires config.yaml in the same directory)

allfinger -u http://target.com -m all
# Full engine scan; default is "fast" for quick scanning. "all" performs a full engine scan with higher CPU usage

allfinger -i 192.168.1.1/24,192.168.2.2
# Scan CIDR range
```

## Parameters

| Parameter | Description |
|-----------|-------------|
| `-i, --cidr` | Scan IP range, e.g., `-i=192.168.1.1/24,192.168.2.1` |
| `-h, --help` | Display help information |
| `-l, --local` | Read assets from a local file for fingerprint recognition |
| `-m, --mode` | Specify scanning engine, default is fast mode (6 engines) |
| `--mysql` | Specify MySQL export output |
| `-o, --output` | Output results, supports json and xlsx formats |
| `-p, --proxy` | Specify proxy for accessing targets |
| `-s, --slient` | Silent output |
| `-t, --thread` | Fingerprint recognition thread pool size (default 100) |
| `-u, --url` | Identify a single target |

## Configuration

Ensure a `config.yaml` file is in the same directory to support MySQL database:

```yaml
database:
  user: "root"
  password: "123456"
  name: "fingers"
  host: "127.0.0.1"
  port: "3307"
```

And pre-create the SQL table:

```sql
CREATE TABLE `port_scan_results` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `domain` text CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci,
  `ip` varchar(45) NOT NULL,
  `port` int(11) NOT NULL,
  `protocol` varchar(10),
  `tls` varchar(10),
  `cdn` varchar(5),
  `cdn_name` text CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci,
  `time` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
);
```

## References

- [ChainReactors Fingers](https://github.com/chainreactors/fingers)
- [Kscan](https://github.com/lcvvvv/kscan/)
- [Ants](https://github.com/panjf2000/ants/)
- [Cobra](https://github.com/spf13/cobra)

---
