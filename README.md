# Web Fingerprint All-in-One

![Allfinger Logo](https://raw.githubusercontent.com/eexp/pic/main/202408201730719.png)

For English version, please refer to [README_EN.md](README_EN.md).
## 介绍

Allfinger 是一款强大的工具，用于识别和记录网页指纹。拥有超过 60,000 个指纹的数据库，提供快速且灵活的扫描能力，是安全专业人士的必备工具。

## 功能

- **广泛的指纹数据库**：超过 60,000 个指纹。
- **快速扫描**：快速扫描目标。
- **灵活的导出选项**：支持多种格式的导出。

## 快速开始

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

## 配置

对于数据库支持（仅 MySQL），请确保在同一目录下有一个 config.yaml 文件，内容如下：

```yaml
database:
  user: "root"
  password: "123456"
  name: "fingers"
  host: "127.0.0.1"
  port: "3307"
```

## 参考

- [https://github.com/chainreactors/fingers](https://github.com/chainreactors/fingers)
- [https://github.com/lcvvvv/kscan/](https://github.com/lcvvvv/kscan/)
- [https://github.com/panjf2000/ants/](https://github.com/panjf2000/ants/)
- [https://github.com/spf13/cobra](https://github.com/spf13/cobra)

