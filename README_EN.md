Certainly! Here's an optimized version of your README:

---

# Web Fingerprint All-in-One

![Allfinger Logo](https://raw.githubusercontent.com/eexp/pic/main/202408201730719.png)

For the Chinese version, please refer to [README.md](README.md).

## Introduction

Allfinger is a powerful tool designed for identifying and recording web fingerprints. With a database of over 60,000 fingerprints, it offers fast and flexible scanning capabilities.

## Features

- **Extensive Fingerprint Database**: Over 60,000 fingerprints.
- **Fast Scanning**: Quickly scan targets.
- **Flexible Export Options**: Supports multiple export formats.

## Quick Start

```bash
allfinger -u http://target.com 
# Scan a single target

allfinger -l /targets.txt
# Scan multiple targets from a file

allfinger -u http://target.com -t 200 
# Set thread count to 200

allfinger -u http://target.com -t 200 -o tg.xlsx
# Export results in json, xlsx, or db format (MySQL supported with config.yaml in the same directory)

allfinger -u http://target.com -m all
# Full engine scan; default is "fast" mode. "All" mode uses more CPU resources

allfinger -i 192.168.1.1/24,192.168.2.2
# Scan a CIDR range
```

### Flags

- `-i, --cidr string` : Scan IP range, e.g., `-i=192.168.1.1/24,192.168.2.1`
- `-h, --help` : Display help for cfingers
- `-l, --local string` : Read assets from a local file for fingerprinting, supports no protocol, e.g., `192.168.1.1:9090 | http://192.168.1.1:9090`
- `-m, --mode string` : Specify scan engine, default is fast mode (6 engines), e.g., `-m=fast`, `-m=all` (default "all")
- `--mysql string` : Specify MySQL export output, e.g., `root:password@tcp(127.0.0.1:3306)/mysql`, or `mysql=config`
- `-o, --output string` : Export results, supports json and xlsx file extensions, and parameters mysql, db, sql, e.g., `-o=db`, `-o=123.xlsx`
- `-p, --proxy string` : Specify proxy for target access, supports http and socks5, e.g., `http://127.0.0.1:8080`, `socks5://127.0.0.1:8080`
- `-s, --slient` : Silent output
- `-t, --thread int` : Set fingerprint recognition thread pool size (default 100)
- `-u, --url string` : Identify a single target

## Configuration

For database support (MySQL only), ensure a `config.yaml` file is in the same directory with the following content:

```yaml
database:
  user: "root"
  password: "123456"
  name: "fingers"
  host: "127.0.0.1"
  port: "3307"
```

## References

- [ChainReactors Fingers](https://github.com/chainreactors/fingers)
- [Kscan](https://github.com/lcvvvv/kscan/)
- [Ants](https://github.com/panjf2000/ants/)
- [Cobra](https://github.com/spf13/cobra)

---

This version is more concise and organized, making it easier for users to understand and use your tool.
