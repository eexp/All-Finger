# Web Fingerprint All-in-One

![Go](https://img.shields.io/badge/Go-1.24+-00ADD8.svg) ![License](https://img.shields.io/badge/License-MIT-blue.svg) ![Version](https://img.shields.io/badge/Version-1.1.16-green.svg)

![](https://raw.githubusercontent.com/eexp/pic/main/202508061632572.gif)

**[中文版本](README.md)**

## Introduction

Allfinger is a powerful web fingerprint identification tool designed to efficiently and accurately identify web technology stacks, CMS platforms, and server information. It features a massive database of over 70,000 fingerprints, supports complex redirect handling, high-precision DOM parsing, and flexible output formats.



## Core Features

- 🚀 **Massive Fingerprint Database**: Contains over 70,000 fingerprints, covering CMS, frameworks, servers, and various technology stacks.
- ⚡ **High-Performance Scanning**: Supports multi-threaded scanning with configurable thread pool size, balancing speed and performance.
- 🔍 **Advanced Redirect Handling**: Supports HTTP 302 redirects, JavaScript redirects, and cookie persistence.
- 🌲 **DOM Tree Optimization**: Improves fingerprint identification accuracy through high-precision DOM parsing and removal of useless content.
- 📤 **Diverse Output Formats**: Supports JSON, XLSX, and MySQL database output to meet different scenario requirements.
- 🛠 **Flexible Scanning Modes**: Provides fast (6 engines) and all (full engine) modes, balancing speed and comprehensiveness.
- 🌐 **Proxy Support**: Supports HTTP and SOCKS5 proxies for complex network environments.
- 📊 **Hash Fingerprinting**: Supports MD5 and MMH3 hash calculation for favicons, enhancing identification capability.
- 🌍 **Browser Mode**: Integrated Chrome browser support, capable of executing JavaScript for accurate SPA application identification.



## Quick Start

Use the following commands to quickly start Allfinger:

```bash
allfinger -u http://target.com 
# Scan a single target

allfinger -u http://target.com -s
# Scan a single target with silent JSON output

allfinger -l /targets.txt
# Scan multiple targets from a file

allfinger -u http://target.com -t 200 
# Set thread count to 200

allfinger -u http://target.com -o tg.xlsx
# Export to json, xlsx, or db format (db currently only supports MySQL, requires config.yaml in the same directory)

allfinger -u http://target.com -m fast
# Full engine scan; default is "all" for full engine scanning with higher CPU usage,
# can switch to "fast" for quick scanning (excludes kscan)

allfinger -i 192.168.1.1/24,192.168.2.2
# Scan CIDR range

```



## Command Line Parameters

| Parameter    | Description                                                  | Example                       |
| ------------ | ------------------------------------------------------------ | ----------------------------- |
| -i, --cidr   | Scan IP range or specific IPs (supports CIDR or comma-separated IP list) | -i=192.168.1.1/24,192.168.2.1 |
| -l, --local  | Read assets from local file (supports URLs or IPs, with or without protocol) | -l=targets.txt                |
| -u, --url    | Scan a single URL or comma-separated URL list                | -u=http://example.com         |
| -t, --thread | Set concurrent thread count (default 100)                    | -t=200                        |
| -m, --mode   | Scan mode: fast (6 engines) or all (full engine)             | -m=fast                       |
| -o, --output | Export results, supports .json, .xlsx or MySQL (db, sql)     | -o=results.xlsx               |
| --mysql      | Export to MySQL, supports DSN or config (reads from config.yaml) | --mysql=config                |
| -p, --proxy  | Specify proxy (supports HTTP or SOCKS5 proxy)                | -p=http://127.0.0.1:8080      |
| -s, --silent | Silent mode, outputs JSON format results only                | -s                            |
| -H, --header | Custom User-Agent header, random UA by default               | -H="Custom UA"                |

---

## Browser Mode

Allfinger integrates Chrome browser support for more accurate identification of modern SPAs (Single Page Applications) and dynamically rendered web content.

### Installing Chrome

```bash
macOS:    brew install --cask google-chrome
Ubuntu:   sudo apt-get install chromium-browser
CentOS:   sudo yum install chromium
Windows:  Download from https://www.google.com/chrome/
```

### Three Operating Modes

- **Normal Mode**: Uses traditional HTTP requests, fast, suitable for static pages
- **Browser Mode**: Uses Chrome to render pages, can execute JavaScript, suitable for dynamic content
- **Auto-Detection Mode**: Intelligently identifies SPA applications and automatically switches to browser mode

### Browser Mode Core Features

- ✅ Full JavaScript code execution
- ✅ Wait for dynamic content loading
- ✅ Screenshot functionality
- ✅ Browser pool management (instance reuse for improved performance)
- ✅ SPA framework auto-detection (Vue, React, Angular, etc.)

### Browser Mode Usage

```bash
# Scan using browser mode
./allfinger -u https://example.com -b

# Auto-detect SPA and switch to browser mode
./allfinger -u https://example.com --auto-browser

# Enable screenshot functionality
./allfinger -u https://example.com -b --screenshot --screenshot-dir ./screens

# Show browser interface (non-headless mode for debugging)
./allfinger -u https://example.com -b --show-browser

# Set page load wait time (seconds)
./allfinger -u https://example.com -b --browser-wait 5

# Set browser pool size (concurrency)
./allfinger -u https://example.com -b --browser-pool 10
```

### Browser Mode Parameter List

| Parameter | Short | Description | Default |
|-----------|-------|-------------|---------|
| `--browser` | `-b` | Use browser mode | false |
| `--auto-browser` | - | Auto-detect SPA applications | false |
| `--show-browser` | - | Show browser interface | false (headless mode) |
| `--browser-ua` | - | Browser User-Agent | Chrome default UA |
| `--browser-timeout` | - | Browser request timeout (seconds) | 30 |
| `--browser-wait` | - | Page load wait time (seconds) | 3 |
| `--browser-pool` | - | Browser pool size | 5 |
| `--screenshot` | - | Enable screenshots | false |
| `--screenshot-dir` | - | Screenshot save directory | ./screenshots |

### Usage Scenario Examples

```bash
# Scenario 1: Scan Vue.js application
./allfinger -u https://vuejs-app.com -b --browser-wait 5

# Scenario 2: Batch scan mixed sites (auto-detection mode intelligently determines browser need)
./allfinger -l targets.txt --auto-browser -t 50

# Scenario 3: Debug mode (show browser interface for viewing page load process)
./allfinger -u https://spa-app.com --show-browser --browser-wait 10

# Scenario 4: Generate screenshot report
./allfinger -l targets.txt -b --screenshot --screenshot-dir ./report/images
```

### SPA Detection Rules

The system automatically detects the following features to determine if it's a SPA application:

1. **Framework Marker Detection**
   - Angular: `ng-app`, `data-ng-app`
   - Vue: `v-app`, `id="app"`
   - React: `id="root"`, `react-root`
   - Next.js: `__NEXT_DATA__`
   - Nuxt.js: `__NUXT__`

2. **Content Ratio Analysis**
   - Large amount of JavaScript code
   - Minimal main HTML content
   - Contains routing component markers

### Performance Optimization Tips

```bash
# Browser mode consumes more resources, recommend reducing thread count appropriately
./allfinger -l large_list.txt -b -t 20 --browser-pool 5

# For fast-loading sites, reduce wait time
./allfinger -u https://fast-site.com -b --browser-wait 1

# For slow-loading sites, increase wait time
./allfinger -u https://slow-site.com -b --browser-wait 10
```

### Large-Scale Scanning Recommended Configuration

```bash
# Recommended config: browser pool size 5-10, thread count 1-2x pool size
./allfinger -l large_targets.txt -b --browser-pool 5 -t 10 --browser-timeout 30

# For very large target lists (>1000), batch scanning is recommended
split -l 500 large_targets.txt batch_
for f in batch_*; do
    ./allfinger -l $f -b --browser-pool 5 -t 10 -o results_$f.json
done
```

### Troubleshooting

| Issue | Solution |
|-------|----------|
| Chrome not found | Install Chrome or Chromium (see installation commands above) |
| Screenshot failed | Check directory permissions: `mkdir -p ./screenshots && chmod 755 ./screenshots` |
| Browser mode timeout | Increase timeout: `--browser-timeout 60` |

### Important Notes

1. **Resource Consumption**: Browser mode consumes more CPU and memory resources; monitor system load during large-scale scanning
2. **Concurrency Limit**: In browser mode, concurrency is automatically limited to 2x browser pool size to prevent resource exhaustion
3. **Timeout Mechanism**: Browser instance acquisition has a 60-second timeout to prevent infinite waiting
4. **Auto Cleanup**: Browser pool is automatically closed after scanning to release system resources

---

## MySQL Configuration

To use MySQL output, ensure a `config.yaml` file exists in the same directory with the following content:

```yaml
database:
  user: "root"
  password: "123456"
  name: "fingers"
  host: "127.0.0.1"
  port: "3307"
```

Pre-create the following table structure in MySQL:

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
) CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci
```



## Example Output

Run the following command:

```
allfinger -u http://example.com -s
```

Sample JSON output:

```
[
  {
    "url": "http://example.com",
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

## Changelog

### 2025-07-09 17:05:13
This update focuses on `kscan` fingerprint matching performance optimization, with the following core changes:

1. **Pre-compiled Regular Expressions**
   - Compiles the 2,814 regular expressions involving `~=` operator during fingerprint library loading.
   - Runtime directly reuses `*regexp.Regexp`, completely eliminating repeated compilation overhead.

2. **Removed `reflect` Reflection Field Reading**
   - Generated dedicated extraction functions for `Title` / `Header` / `Body` fields, cached in `Param`.
   - Zero reflection, zero allocation at runtime, significantly reducing CPU and GC pressure.

3. **Expression Parsing Upgraded to AST + Short-Circuit Evaluation**
   - Parses logical expressions into Boolean Syntax Tree (AST) during loading.
   - Runtime direct node recursive matching, supports `&&` / `||` short-circuit.

4. **Preserved Original Logic Priority**
   - `&&` and `||` set to same level priority, maintaining old version left-associative behavior to avoid old fingerprint mismatches.

5. **Performance Benefits**
   - In full matching scenarios with 24,265 fingerprints, single request CPU time expected to decrease by 1-2 orders of magnitude (hardware dependent).
   - Memory usage slightly increased (approximately +5 MiB), in exchange for several times throughput improvement.

> These optimizations require no changes to any external API calls; all existing use cases can directly benefit from performance acceleration.
