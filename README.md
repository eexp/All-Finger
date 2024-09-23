---

# Web Fingerprint All-in-One

![Allfinger Logo](https://raw.githubusercontent.com/eexp/pic/main/202408201730719.png)

[English Version](README_EN.md)

## 介绍

**Allfinger** 是一款功能强大的网页指纹识别工具，拥有超过 60,000 个指纹的数据库，提供快速且灵活的扫描能力。

## 功能亮点

- 🚀 **广泛的指纹数据库**：超过 60,000 个指纹，覆盖全面。
- ⚡ **快速扫描**：高效的扫描速度，节省时间。
- 📂 **灵活的导出选项**：支持多种格式的导出，满足不同需求。

## 快速开始

使用以下命令快速启动 Allfinger：

```bash
allfinger -u http://target.com 
# 扫描单个目标

allfinger -l /targets.txt
# 从文件中扫描多个目标

allfinger -u http://target.com -t 200 
# 设置线程数为 200

allfinger -u http://target.com -t 200 -o tg.xlsx
# 支持 json, xlsx, db 格式导出（目前 db 仅支持 MySQL，需要在同一目录下配置 config.yaml）

allfinger -u http://target.com -m all
# 全引擎扫描；默认是“fast”快速扫描。“all”进行全引擎扫描，CPU 占用较高

allfinger -i 192.168.1.1/24,192.168.2.2
# 扫描 CIDR 范围
```

## 参数说明

| 参数 | 描述 |
|------|------|
| `-i, --cidr` | 扫描 IP 段，例如：`-i=192.168.1.1/24,192.168.2.1` |
| `-h, --help` | 显示帮助信息 |
| `-l, --local` | 从本地文件读取资产进行指纹识别 |
| `-m, --mode` | 指定扫描引擎，默认快速模式（6个引擎） |
| `--mysql` | 指定 MySQL 导出输出结果 |
| `-o, --output` | 输出结果，支持 json 和 xlsx 格式 |
| `-p, --proxy` | 指定访问目标时的代理 |
| `-s, --slient` | 静默输出 |
| `-t, --thread` | 指纹识别线程池大小（默认 100） |
| `-u, --url` | 识别单个目标 |

## 配置

确保在同一目录下有一个 `config.yaml` 文件以支持 MySQL 数据库：

```yaml
database:
  user: "root"
  password: "123456"
  name: "fingers"
  host: "127.0.0.1"
  port: "3307"
```

并预先创建 SQL 表：

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

## 参考资料

- [ChainReactors Fingers](https://github.com/chainreactors/fingers)
- [Kscan](https://github.com/lcvvvv/kscan/)
- [Ants](https://github.com/panjf2000/ants/)
- [Cobra](https://github.com/spf13/cobra)

---
