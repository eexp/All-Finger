# Web Fingerprint All-in-One
![](https://raw.githubusercontent.com/eexp/pic/main/202408201730719.png)
## Introduction

Allfinger is a powerful tool designed to identify and catalog web fingerprints. With a database of over 60,000 fingerprints, it provides fast and flexible scanning capabilities, making it an essential tool for security professionals.
## Features

- Extensive Fingerprint Database: Over 60,000 fingerprints available.
- Fast Scanning: Quickly scan your targets.
- Flexible Export Options: Supports export in various formats.

## Quick start

```
allfinger -u http://target.com 
# Scan a single target

allfinger -l /targets.txt
# Scan multiple targets from a file

allfinger -u http://target.com -t 200 
# Set thread count to 200

allfinger -u http://target.com -t 200 -o tg.xlsx
# Supports json, xlsx, db (Currently db only supports MySQL, configuration needed in the same directory with config.yaml)

allfinger -u http://target.com -m all
# Full-engine scan; the default is "fast" for a quick scan. "all" for full-engine scan which is CPU intensive

allfinger -i 192.168.1.1/24,192.168.2.2
# Scan CIDR ranges

……

```



## Configuration
For database support(only mysql), ensure you have a config.yaml file in the same directory with the following content:

```<YAML>
database:
  user: "root"
  password: "123456"
  name: "fingers"
  host: "127.0.0.1"
  port: "3307"
```


## Reference

- https://github.com/chainreactors/fingers
- https://github.com/lcvvvv/kscan/
- https://github.com/panjf2000/ants/
- https://github.com/spf13/cobra
- 
