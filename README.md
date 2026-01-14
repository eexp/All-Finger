# Web Fingerprint All-in-One

![Go](https://img.shields.io/badge/Go-1.24+-00ADD8.svg) ![License](https://img.shields.io/badge/License-MIT-blue.svg) ![Version](https://img.shields.io/badge/Version-1.1.21-green.svg)

![](https://raw.githubusercontent.com/eexp/pic/main/202508061632572.gif)

**[English Version](README_EN.md)**

## 介绍

Allfinger 是一款功能强大的网页指纹识别工具，旨在高效、精准地识别网页技术栈、CMS 平台和服务器信息。它拥有超过 75,000 个指纹的庞大数据库，支持复杂的跳转处理、高精度 DOM 解析以及灵活的输出格式。



## 核心功能

- 🚀 **海量指纹数据库**：包含超过 70,000 个指纹，覆盖 CMS、框架、服务器等多种技术栈。
- ⚡ **高效扫描**：支持多线程扫描，线程池大小可配置，兼顾速度与性能。
- 🔍 **高级跳转处理**：支持 HTTP 302 跳转、JavaScript 跳转以及 Cookie 携带。
- 🌲 **DOM 树精简**：通过高精度 DOM 解析和无用内容去除，提升指纹识别准确性。
- 📤 **多样化输出**：支持 JSON、XLSX 和 MySQL 数据库输出，满足不同场景需求。
- 🛠 **灵活扫描模式**：提供 fast（6 个引擎）和 all（全引擎）两种模式，兼顾速度与全面性。
- 🌐 **代理支持**：支持 HTTP 和 SOCKS5 代理，适配复杂网络环境。
- 📊 **哈希指纹**：支持 favicon 的 MD5 和 MMH3 哈希计算，增强识别能力。
- 🌍 **浏览器模式**：集成 Chrome 浏览器支持，可执行 JavaScript，精准识别 SPA 应用。



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
# 全引擎扫描；默认是"all"进行全引擎扫描，CPU 占用较高,
# 可以灵活切换"fast"快速扫描。「不包含kscan」

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

---

## 浏览器模式

Allfinger 集成了 Chrome 浏览器支持，能够更准确地识别现代 SPA（单页应用）和动态渲染的网页内容。

### 安装 Chrome

```bash
macOS:    brew install --cask google-chrome
Ubuntu:   sudo apt-get install chromium-browser
CentOS:   sudo yum install chromium
Windows:  下载 https://www.google.com/chrome/
```

### 三种工作模式

- **普通模式**：使用传统 HTTP 请求，速度快，适合静态页面
- **浏览器模式**：使用 Chrome 渲染页面，可执行 JavaScript，适合动态内容
- **自动检测模式**：智能识别 SPA 应用，自动切换到浏览器模式

### 浏览器模式核心功能

- ✅ 完整执行 JavaScript 代码
- ✅ 等待动态内容加载
- ✅ 截图功能
- ✅ 浏览器池管理（复用实例，提高性能）
- ✅ SPA 框架自动检测（Vue、React、Angular 等）

### 浏览器模式使用方法

```bash
# 使用浏览器模式扫描
./allfinger -u https://example.com -b

# 自动检测 SPA 并切换到浏览器模式
./allfinger -u https://example.com --auto-browser

# 启用截图功能
./allfinger -u https://example.com -b --screenshot --screenshot-dir ./screens

# 显示浏览器界面（非无头模式，用于调试）
./allfinger -u https://example.com -b --show-browser

# 设置页面加载等待时间（秒）
./allfinger -u https://example.com -b --browser-wait 5

# 设置浏览器池大小（并发数）
./allfinger -u https://example.com -b --browser-pool 10
```

### 浏览器模式参数列表

| 参数 | 简写 | 说明 | 默认值 |
|------|------|------|--------|
| `--browser` | `-b` | 使用浏览器模式 | false |
| `--auto-browser` | - | 自动检测 SPA 应用 | false |
| `--show-browser` | - | 显示浏览器界面 | false（无头模式） |
| `--browser-ua` | - | 浏览器 User-Agent | Chrome 默认 UA |
| `--browser-timeout` | - | 浏览器请求超时（秒） | 30 |
| `--browser-wait` | - | 等待页面加载时间（秒） | 3 |
| `--browser-pool` | - | 浏览器池大小 | 5 |
| `--screenshot` | - | 启用截图 | false |
| `--screenshot-dir` | - | 截图保存目录 | ./screenshots |

### 使用场景示例

```bash
# 场景 1：扫描 Vue.js 应用
./allfinger -u https://vuejs-app.com -b --browser-wait 5

# 场景 2：批量扫描混合站点（自动检测模式会智能判断是否需要浏览器）
./allfinger -l targets.txt --auto-browser -t 50

# 场景 3：调试模式（显示浏览器界面，方便查看页面加载过程）
./allfinger -u https://spa-app.com --show-browser --browser-wait 10

# 场景 4：生成截图报告
./allfinger -l targets.txt -b --screenshot --screenshot-dir ./report/images
```

### SPA 检测规则

系统会自动检测以下特征来判断是否为 SPA 应用：

1. **框架标记检测**
   - Angular: 
   - Vue: 
   - React: 
   - Next.js: 
   - Nuxt.js: 
   - ……

2. **内容比例分析**
   - 大量 JavaScript 代码
   - 主体 HTML 内容很少
   - 包含路由组件标记

### 性能优化建议

```bash
# 浏览器模式消耗资源较多，建议适当降低线程数
./allfinger -l large_list.txt -b -t 20 --browser-pool 5

# 对于加载较快的站点，减少等待时间
./allfinger -u https://fast-site.com -b --browser-wait 1

# 对于加载较慢的站点，增加等待时间
./allfinger -u https://slow-site.com -b --browser-wait 10
```

### 大规模扫描建议配置

```bash
# 推荐配置：浏览器池大小 5-10，线程数为池大小的 1-2 倍
./allfinger -l large_targets.txt -b --browser-pool 5 -t 10 --browser-timeout 30

# 对于非常大的目标列表（>1000），建议分批扫描
split -l 500 large_targets.txt batch_
for f in batch_*; do
    ./allfinger -l $f -b --browser-pool 5 -t 10 -o results_$f.json
done
```

### 故障排除

| 问题 | 解决方案 |
|------|----------|
| Chrome 未找到 | 安装 Chrome 或 Chromium（见上方安装命令） |
| 截图失败 | 检查目录权限：`mkdir -p ./screenshots && chmod 755 ./screenshots` |
| 浏览器模式超时 | 增加超时时间：`--browser-timeout 60` |

### 注意事项

1. **资源占用**：浏览器模式会消耗更多的 CPU 和内存资源，大规模扫描时请注意系统负载
2. **并发限制**：浏览器模式下，并发数会自动限制为浏览器池大小的 2 倍，避免资源耗尽
3. **超时机制**：获取浏览器实例设有 60 秒超时，避免无限等待导致卡死
4. **自动清理**：扫描结束后会自动关闭浏览器池，释放系统资源

---

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
