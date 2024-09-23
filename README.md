# Web Fingerprint All-in-One

![Allfinger Logo](https://raw.githubusercontent.com/eexp/pic/main/202408201730719.png)

For English version, please refer to [README_EN.md](README_EN.md).
## 介绍

Allfinger 是一款强大的工具，用于识别和记录网页指纹。拥有超过 60,000 个指纹的数据库，提供快速且灵活的扫描能力。

## 功能

- **广泛的指纹数据库**：超过 60,000 个指纹。
- **快速扫描**：超快的扫描速度。
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

## 参数说明
```
Flags:
  -i, --cidr string     扫描ip段，例如：-i=192,168.1.1/24,192.168.2.1
  -h, --help            help for cfingers
  -l, --local string    从本地文件读取资产，进行指纹识别，支持无协议，例如：192.168.1.1:9090 | http://192.168.1.1:9090
  -m, --mode string     指定扫描时候引擎，默认快速模式（6个引擎），例如：-m=fast,-m=all (default "all")
      --mysql string    指定mysql导出输出结果,root:password@tcp(127.0.0.1:3306)/mysql,或者config导入,mysql=config
  -o, --output string   输出所有结果，支持json和xlsx后缀的文件。以及参数mysql,db,sql。例如 -o=db ,-o=123.xlsx
  -p, --proxy string    指定访问目标时的代理，支持http代理和socks5，例如：http://127.0.0.1:8080、socks5://127.0.0.1:8080
  -s, --slient          静默输出
  -t, --thread int      指纹识别线程池大小。 (default 100)
  -u, --url string      识别单个目标。
```

## 配置

对于数据库支持（仅 MySQL），请确保在同一目录下有一个 config.yaml 文件，以及预先创建好sql，内容如下：

```yaml
database:
  user: "root"
  password: "123456"
  name: "fingers"
  host: "127.0.0.1"
  port: "3307"
```

```sql
CREATE TABLE `port_scan_results` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `domain` text CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci,
  `ip` varchar(45) NOT NULL,
  `port` int(11) NOT NULL,
  `protocol` varchar(10) ,
  `tls` varchar(10),
  `cdn` varchar(5),           
  `cdn_name` text CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci,
  `time` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
);
```

## 参考

- [https://github.com/chainreactors/fingers](https://github.com/chainreactors/fingers)
- [https://github.com/lcvvvv/kscan/](https://github.com/lcvvvv/kscan/)
- [https://github.com/panjf2000/ants/](https://github.com/panjf2000/ants/)
- [https://github.com/spf13/cobra](https://github.com/spf13/cobra)

