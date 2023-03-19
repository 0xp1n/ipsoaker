![computer_basement_logo](assets/computer_basement.jpg)

`Ipharvest` is a command-line tool for extracting IP addresses from various data sources and generate reports in multiple formats such as csv and json.

- [Installation](#installation)
  - [Requirements](#requirements)
  - [Compile the binary for your system](#compile-the-binary-for-your-system)
    - [**Important:**](#important)
- [License](#license)
- [Usage](#usage)
  - [Output result into a file](#output-result-into-a-file)
  - [Examples](#examples)
    - [Extract IP addresses from a local text file:](#extract-ip-addresses-from-a-local-text-file)
    - [Extract IP addreses by selecting the mode](#extract-ip-addreses-by-selecting-the-mode)
    - [Extract IPv6 addresses from a raw text:](#extract-ipv6-addresses-from-a-raw-text)
    - [Extract IP addresses from a remote URL and geolocate them:](#extract-ip-addresses-from-a-remote-url-and-geolocate-them)
    - [Extract IPv6 addresses from a local file and save the report as a JSON file:](#extract-ipv6-addresses-from-a-local-file-and-save-the-report-as-a-json-file)
    - [Extract IP addresses from a local log file, geolocate them, and save the report as a CSV file:](#extract-ip-addresses-from-a-local-log-file-geolocate-them-and-save-the-report-as-a-csv-file)
- [Report examples](#report-examples)
  - [RAW](#raw)
  - [JSON without geolocation](#json-without-geolocation)
  - [JSON with geolocation](#json-with-geolocation)
  - [CSV without geolocation](#csv-without-geolocation)
  - [CSV with geolocation](#csv-with-geolocation)

# Installation

## Requirements

- Bash version 4+
- (Optional) If you want to save reports on **.json** format you need the library [jq](https://github.com/stedolan/jq) installed in your system

```bash
# MacOS
brew install jq

# GNU/Linux
sudo apt install jq
```

---

You just need the binaries to start using the tool, replace `<architecture>` on the url to your desired architecture. If you want to have global access in your shell move the binary into your selected binary path from `$PATH variable`, usually `/usr/bin or /usr/local/bin`

```bash
# Template for Curl | Wget
curl -X GET https://github.com/0xp1n/ipharvest/blob/main/bin/<architecture>/ipharvest
wget https://github.com/0xp1n/ipharvest/blob/main/bin/<architecture>/ipharvest

# ARM64 (MacOS with Apple silicon)
curl -X GET https://github.com/0xp1n/ipharvest/blob/main/bin/arm64/ipharvest
wget https://github.com/0xp1n/ipharvest/blob/main/bin/arm64/ipharvest

# x64-64 (GNU/Linux)
curl -X GET https://github.com/0xp1n/ipharvest/blob/main/bin/x86-64/ipharvest
wget https://github.com/0xp1n/ipharvest/blob/main/bin/x86-64/ipharvest

## Move the binary to some directory defined on global variable $PATH to have it globally available in terminal
mv ipharvest /usr/bin
```

## Compile the binary for your system

In case none of the architectures provided fit for the requirements of your system just download the project and run the script located on `build/compile.sh`.

### **Important:**

Make sure you change the shebang `#!/opt/homebrew/bin/bash` on script `ipharvest.sh` according for your bash shell, that's usually `#!/bin/bash`. Don't use the dynamic `#!/usr/bin/env bash` because the `shc` library will not be able to compile the script.

You will found the compiled binary inside `bin/` folder

```bash
git clone https://github.com/0xp1n/ipharvest.git

# Change the shebang `#!/bin/bash`
bash build/compile.sh

./bin/ipharvest
```

# License

This tool is licensed under the [GNU General Public License v3.0](LICENSE.md)

# Usage

ipharvest [OPTIONS] [--]

```bash
Options

    -s, --source:       Choose the source data to extract IPs from.
    -m, --mode <type>:  Choose the mode of extraction (ipv4, ipv6, or both).
        --geolocation:  Geolocate all the IP matches.
    -o, --output:       Define a file path to save the report generated by the tool in plain text (also supported JSON and CSV).
    -v, --version:      Display the actual version.
    -h, --help:         Print help information.

Default arguments:
    mode: both
    geolocation: off
    output: off
```

## Output result into a file

To choose the format of the outupt just set the format in the path like `documents/report.json` or `report.csv`. The tool automatically detects the format to make the proper transformations. If no format is provided it will be saved as raw text.

## Examples

### Extract IP addresses from a local text file:

```bash
ipharvest --source data.txt
ipharvest -s data.txt
```

### Extract IP addreses by selecting the mode

This option can speed up the process if you know that the source only contains ipv4 addreses for example.
Supported modes: `ipv4,ipv6,both`

```bash
ipharvest -s ipv4.log -m ipv4
```

### Extract IPv6 addresses from a raw text:

The text does not need to have a specific separator between ips, the tool is prepared to harvest in all the information noise that surrounds them

```bash
ipharvest -s "192.168.1.1/24,10.10.10.25,2404:6800:4008:c02::8b" --mode ipv6
```

### Extract IP addresses from a remote URL and geolocate them:

```bash
ipharvest -s https://example.com/log.txt --geolocation
ipharvest -s http://localhost:8000/short_ipv4.dat -m ipv4 -o report_geo.json --geolocation
```

### Extract IPv6 addresses from a local file and save the report as a JSON file:

```bash
ipharvest -s ipv6.txt --mode ipv6 -o documents/reports/ipv6.json
```

### Extract IP addresses from a local log file, geolocate them, and save the report as a CSV file:

```bash

ipharvest -source /var/log/example.log --geolocation --mode both -o reports/ip_harvest.csv
```

# Report examples

## RAW

```txt
IP-ADDRESS      COUNT  COUNTRY        LATITUDE  LONGITUDE  TIMEZONE          ISP
96.30.52.60     1      United_States  37.751    -97.822    America/Chicago   LIQUIDWEB
95.211.75.10    1      Netherlands    52.3824   4.8995     Europe/Amsterdam  LeaseWeb_Netherlands_B.V.
94.130.109.30   1      Germany        51.2993   9.491      Europe/Berlin     Hetzner_Online_GmbH
92.53.96.22     1      Russia         59.9417   30.3096    Europe/Moscow     TimeWeb_Ltd.
104.24.109.92   1      United_States  32.7889   -96.8021   America/Chicago   CLOUDFLARENET
104.24.108.92   1      United_States  32.7889   -96.8021   America/Chicago   CLOUDFLARENET
104.24.103.57   1      United_States  32.7889   -96.8021   America/Chicago   CLOUDFLARENET
104.24.102.57   1      United_States  32.7889   -96.8021   America/Chicago   CLOUDFLARENET
104.171.23.70   1      United_States  37.751    -97.822    America/Chicago   SPRINTLINK
104.171.23.69   1      United_States  37.751    -97.822    America/Chicago   SPRINTLINK
104.164.181.36  1      United_States  37.751    -97.822    America/Chicago   EGIHOSTING
103.70.226.182  1      China          34.7732   113.722    Asia/Shanghai     LEMON_TELECOMMUNICATIONS_LIMITED
```

## JSON without geolocation

```json
[
  {
    "IP-ADDRESS": "35.186.238.101",
    "COUNT": "4"
  },
  {
    "IP-ADDRESS": "23.236.62.147",
    "COUNT": "4"
  },
  {
    "IP-ADDRESS": "112.78.125.29",
    "COUNT": "3"
  },
  {
    "IP-ADDRESS": "96.30.52.60",
    "COUNT": "1"
  },
  {
    "IP-ADDRESS": "95.211.75.10",
    "COUNT": "1"
  }
]
```

## JSON with geolocation

```json
[
  {
    "IP-ADDRESS": "96.30.52.60",
    "COUNT": "1",
    "COUNTRY": "United_States",
    "LATITUDE": "37.751",
    "LONGITUDE": "-97.822",
    "TIMEZONE": "America/Chicago",
    "ISP": "LIQUIDWEB"
  },
  {
    "IP-ADDRESS": "95.211.75.10",
    "COUNT": "1",
    "COUNTRY": "Netherlands",
    "LATITUDE": "52.3824",
    "LONGITUDE": "4.8995",
    "TIMEZONE": "Europe/Amsterdam",
    "ISP": "LeaseWeb_Netherlands_B.V."
  },
  {
    "IP-ADDRESS": "94.130.109.30",
    "COUNT": "1",
    "COUNTRY": "Germany",
    "LATITUDE": "51.2993",
    "LONGITUDE": "9.491",
    "TIMEZONE": "Europe/Berlin",
    "ISP": "Hetzner_Online_GmbH"
  },
  {
    "IP-ADDRESS": "92.53.96.22",
    "COUNT": "1",
    "COUNTRY": "Russia",
    "LATITUDE": "59.9417",
    "LONGITUDE": "30.3096",
    "TIMEZONE": "Europe/Moscow",
    "ISP": "TimeWeb_Ltd."
  }
]
```

## CSV without geolocation

```csv
IP-ADDRESS,COUNT,COUNTRY,LATITUDE,LONGITUDE,TIMEZONE,ISP
96.30.52.60,1,,,,,
95.211.75.10,1,,,,,
94.130.109.30,1,,,,,
92.53.96.22,1,,,,,
104.24.109.92,1,,,,,
104.24.108.92,1,,,,,
104.24.103.57,1,,,,,
104.24.102.57,1,,,,,
104.171.23.70,1,,,,,
104.171.23.69,1,,,,,
104.164.181.36,1,,,,,
103.70.226.182,1,,,,,
```

## CSV with geolocation

```csv
IP-ADDRESS,COUNT,COUNTRY,LATITUDE,LONGITUDE,TIMEZONE,ISP
96.30.52.60,1,United_States,37.751,-97.822,America/Chicago,LIQUIDWEB
95.211.75.10,1,Netherlands,52.3824,4.8995,Europe/Amsterdam,LeaseWeb_Netherlands_B.V.
94.130.109.30,1,Germany,51.2993,9.491,Europe/Berlin,Hetzner_Online_GmbH
92.53.96.22,1,Russia,59.9417,30.3096,Europe/Moscow,TimeWeb_Ltd.
104.24.109.92,1,United_States,32.7889,-96.8021,America/Chicago,CLOUDFLARENET
104.24.108.92,1,United_States,32.7889,-96.8021,America/Chicago,CLOUDFLARENET
104.24.103.57,1,United_States,32.7889,-96.8021,America/Chicago,CLOUDFLARENET
104.24.102.57,1,United_States,32.7889,-96.8021,America/Chicago,CLOUDFLARENET
104.171.23.70,1,United_States,37.751,-97.822,America/Chicago,SPRINTLINK
104.171.23.69,1,United_States,37.751,-97.822,America/Chicago,SPRINTLINK
104.164.181.36,1,United_States,37.751,-97.822,America/Chicago,EGIHOSTING
103.70.226.182,1,China,34.7732,113.722,Asia/Shanghai,LEMON_TELECOMMUNICATIONS_LIMITED

```
