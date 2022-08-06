# dsnet-nsupdate

A script to maintain an up-to-date DNS zone based on a dsnet centralised wireguard VPN. It does this by comparing what is currently in DNS (aided by creating a list of peers in a TXT record in the DNS zone), compared with what needs to be in DNS based on the output of `dsnet report`, `dsnetreport.json`. It supports both forward and reverse records for IPv4 and IPv6, and can also update an external nameserver in a split-horizon configuration.

## Requirements
A working BIND configuration that allows dynamic updates with TSIG keys

See also: `requirements.txt`

## Background

The script uses a TXT record in the zone to maintain a list of what records have been placed there by it. Each time it is run, it queries this list, and then performs more queries on each hostname in this list to determine what is currently in DNS. It then parses`dsnetreport.json` to determine what SHOULD be in DNS. It then compares the two and updates as neccessary via TSIG authenticated dynamic updates.

The default TTL for entries maintained by this script is 300. If whilst comparing data in DNS it finds an entry with a TTL of over this, it will assume that this is taken by something else and will assign a '-dsnet' suffix to the hostname before putting it in DNS. It will also determine if a subzone has been delegated to a peer (by way of an NS record for that hostname) and ignore it if this is the case.

## Usage

The majority of data is obtained from `dsnetreport.json`, but can be overridden by specifiying in a config file or via environment variables. It can be run with no arguments, defaulting to the config file `./config.yaml`, or with a config file as it's only argument, or entirely with environment variables. Or even a combination of all of the above. Environment variables take precedent over the configuration file, which takes precedent over the defaults.

The only information that has no defaults and MUST be given are the DNS TSIG keys.

## Configuration

TO BE ADDED

| YAML key | Description | Optional | Default | Environment variable |
| :--- | :--- | :--- | :--- | :--- |

