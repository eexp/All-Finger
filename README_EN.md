# Web Fingerprint All-in-One

![Go](https://img.shields.io/badge/Go-1.18+-00ADD8.svg) ![License](https://img.shields.io/badge/License-MIT-blue.svg) ![Version](https://img.shields.io/badge/Version-1-green.svg)

![](https://raw.githubusercontent.com/eexp/pic/main/202505261718921.png)

**[中文版](README.md)**

## Introduction

Allfinger is a powerful web fingerprinting tool designed for efficient and accurate identification of web technology stacks, CMS platforms, and server information. It boasts a massive database of over 70,000 fingerprints and supports complex redirect handling, high-precision DOM parsing, and flexible output formats.

## Core Features

- 🚀 **Massive Fingerprint Database**: Contains over 70,000 fingerprints, covering CMS, frameworks, servers, and various other technology stacks.
- ⚡ **Efficient Scanning**: Supports multi-threaded scanning with a configurable thread pool size, balancing speed and performance.
- 🔍 **Advanced Redirect Handling**: Supports HTTP 302 redirects, JavaScript redirects, and cookie persistence.
- 🌲 **DOM Tree Simplification**: Enhances fingerprinting accuracy through high-precision DOM parsing and removal of irrelevant content.
- 📤 **Diverse Output Options**: Supports JSON, XLSX, and MySQL database output to meet various needs.
- 🛠 **Flexible Scanning Modes**: Offers `fast` (6 engines) and `all` (all engines) modes to balance speed and comprehensiveness.
- 🌐 **Proxy Support**: Supports HTTP and SOCKS5 proxies to adapt to complex network environments.
- 📊 **Hash Fingerprints**: Supports MD5 and MMH3 hash calculations for favicons, enhancing identification capabilities.

## Quick Start

Use the following commands to quickly start Allfinger:

```bash
allfinger -u [http://target.com](http://target.com)
# Scan a single target

allfinger -u [http://target.com](http://target.com) -s
# Scan a single target and output in silent JSON format

allfinger -l /targets.txt
# Scan multiple targets from a file

allfinger -u [http://target.com](http://target.com) -t 200
# Set the number of threads to 200

allfinger -u [http://target.com](http://target.com) -o tg.xlsx
# Export results in json, xlsx, or db format (currently db only supports MySQL, requires config.yaml in the same directory)

allfinger -u [http://target.com](http://target.com) -m fast
# Full engine scan; default is "all" for full engine scan, which has higher CPU usage.
# You can flexibly switch to "fast" for a quick scan (does not include kscan).

allfinger -i 192.168.1.1/24,192.168.2.2
# Scan a CIDR range
```

## Command Line Arguments

| Parameter     | Description                                                                 | Example                       |
|---------------|-----------------------------------------------------------------------------|-------------------------------|
| -i, --cidr    | Scan IP range or specific IPs (supports CIDR or comma-separated IP list)    | -i=192.168.1.1/24,192.168.2.1 |
| -l, --local   | Read assets from a local file (supports URL or IP, supports protocol-less format) | -l=targets.txt                |
| -u, --url     | Scan a single URL or a comma-separated list of URLs                         | -u=http://example.com         |
| -t, --thread  | Set the number of concurrent threads (default 100)                          | -t=200                        |
| -m, --mode    | Scan mode: `fast` (6 engines) or `all` (all engines)                        | -m=fast                       |
| -o, --output  | Export results, supports .json, .xlsx, or MySQL (db, sql)                   | -o=results.xlsx               |
| --mysql       | Export to MySQL, supports DSN or `config` (reads configuration from config.yaml) | --mysql=config                |
| -p, --proxy   | Specify a proxy (supports HTTP or SOCKS5 proxy)                             | -p=http://127.0.0.1:8080      |
| -s, --silent  | Silent mode, only outputs results in JSON format                            | -s                            |
| -H, --header  | Custom User-Agent header, defaults to a random UA                           | -H="Custom UA"                |

## MySQL Configuration

If using MySQL output, ensure you have a `config.yaml` file in the same directory with the following content:

```yaml
database:
  user: "root"
  password: "123456"
  name: "fingers"
  host: "127.0.0.1"
  port: "3307"
```

And create the following table structure in MySQL beforehand:

```sql
CREATE TABLE IF NOT EXISTS finger_scan_results (
    id INT AUTO_INCREMENT PRIMARY KEY,
    url TEXT CHARACTER SET utf8mb4,
    finger TEXT CHARACTER SET utf8mb4,
    server TEXT CHARACTER SET utf8mb4,
    statuscode INT,
    bodylength INT,
    title TEXT CHARACTER SET utf8mb4,
    time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    md5hash VARCHAR(32),
    mmh3hash VARCHAR(32)
) CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
```

## Example Output

Run the following command:

```bash
allfinger -u [http://example.com](http://example.com) -s
```

Generated JSON output example:

```json
[
  {
    "url": "[http://example.com](http://example.com)",
    "cms": "WordPress",
    "server": "Apache/2.4.41",
    "statuscode": 200,
    "length": 1024,
    "title": "Example Domain",
    "md5hash": "d41d8cd98f00b204e9800998ecf8427e",
    "mmh3hash": "123456789"
  }
]
```

![](https://raw.githubusercontent.com/eexp/pic/main/202505261728670.png)

## References

- [ChainReactors Fingers](https://github.com/chainreactors/fingers)
- [Kscan](https://github.com/lcvvvv/kscan/)
- [Ants](https://github.com/panjf2000/ants/)
- [Cobra](https://github.com/spf13/cobra)

---
