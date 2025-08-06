# Web Fingerprint All-in-One

![Go](https://img.shields.io/badge/Go-1.24+-00ADD8.svg) ![License](https://img.shields.io/badge/License-MIT-blue.svg) ![Version](https://img.shields.io/badge/Version-1.1.16-green.svg)

![](https://raw.githubusercontent.com/eexp/pic/main/202508061632572.gif)

**[English Version](README_EN.md)**

## 介绍

Allfinger 是一款功能强大的网页指纹识别工具，旨在高效、精准地识别网页技术栈、CMS 平台和服务器信息。它拥有超过 70,000 个指纹的庞大数据库，支持复杂的跳转处理、高精度 DOM 解析以及灵活的输出格式。



## 核心功能

- 🚀 **海量指纹数据库**：包含超过 70,000 个指纹，覆盖 CMS、框架、服务器等多种技术栈。
- ⚡ **高效扫描**：支持多线程扫描，线程池大小可配置，兼顾速度与性能。
- 🔍 **高级跳转处理**：支持 HTTP 302 跳转、JavaScript 跳转以及 Cookie 携带。
- 🌲 **DOM 树精简**：通过高精度 DOM 解析和无用内容去除，提升指纹识别准确性。
- 📤 **多样化输出**：支持 JSON、XLSX 和 MySQL 数据库输出，满足不同场景需求。
- 🛠 **灵活扫描模式**：提供 fast（6 个引擎）和 all（全引擎）两种模式，兼顾速度与全面性。
- 🌐 **代理支持**：支持 HTTP 和 SOCKS5 代理，适配复杂网络环境。
- 📊 **哈希指纹**：支持 favicon 的 MD5 和 MMH3 哈希计算，增强识别能力。



## 快速开始

使用以下命令快速启动 Allfinger：

```bash
allfinger -u http://target.com 
# 扫描单个目标

allfinger -u http://target.com -s
# 扫描单个目标 静默输出json格式

allfinger -l /targets.txt
# 从文件中扫描多个目标

allfinger -u http://target.com -t 200 
# 设置线程数为 200

allfinger -u http://target.com -o tg.xlsx
# 支持 json, xlsx, db 格式导出（目前 db 仅支持 MySQL，需要在同一目录下配置 config.yaml）

allfinger -u http://target.com -m fast
# 全引擎扫描；默认是“all”进行全引擎扫描，CPU 占用较高,
# 可以灵活切换“fast”快速扫描。「不包含kscan」

allfinger -i 192.168.1.1/24,192.168.2.2
# 扫描 CIDR 范围

```



## 命令行参数

| 参数         | 描述                                                        | 示例                          |
| ------------ | ----------------------------------------------------------- | ----------------------------- |
| -i, --cidr   | 扫描 IP 段或特定 IP（支持 CIDR 或逗号分隔的 IP 列表）       | -i=192.168.1.1/24,192.168.2.1 |
| -l, --local  | 从本地文件读取资产（支持 URL 或 IP，支持无协议格式）        | -l=targets.txt                |
| -u, --url    | 扫描单个 URL 或逗号分隔的 URL 列表                          | -u=http://example.com         |
| -t, --thread | 设置并发线程数（默认 100）                                  | -t=200                        |
| -m, --mode   | 扫描模式：fast（6 个引擎）或 all（全引擎）                  | -m=fast                       |
| -o, --output | 导出结果，支持 .json、.xlsx 或 MySQL（db、sql）             | -o=results.xlsx               |
| --mysql      | 导出到 MySQL，支持 DSN 或 config（从 config.yaml 读取配置） | --mysql=config                |
| -p, --proxy  | 指定代理（支持 HTTP 或 SOCKS5 代理）                        | -p=http://127.0.0.1:8080      |
| -s, --silent | 静默模式，仅输出 JSON 格式结果                              | -s                            |
| -H, --header | 自定义 User-Agent 头，默认随机 UA                           | -H="Custom UA"                |

## MySQL 配置

若使用 MySQL 输出，确保在同一目录下有一个 `config.yaml` 文件，内容如下：

```yaml
database:
  user: "root"
  password: "123456"
  name: "fingers"
  host: "127.0.0.1"
  port: "3307"
```

并在 MySQL 中预先创建以下表结构：

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



## 示例输出

运行以下命令：

```
allfinger -u http://example.com -s
```

生成的 JSON 输出示例：

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

## 参考

- [ChainReactors Fingers](https://github.com/chainreactors/fingers)
- [Kscan](https://github.com/lcvvvv/kscan/)
- [Ants](https://github.com/panjf2000/ants/)
- [Cobra](https://github.com/spf13/cobra)

---
