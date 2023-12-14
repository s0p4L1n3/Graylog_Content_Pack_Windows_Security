# Windows Security Content Pack for Graylog

Tested with Winlogbeat & Filebeat 7.12.1.0 / Windows 2022 / Windows 10 / Graylog 5.2.2

The Content Pack should be compatible with all Graylog 5.2.X version. 
This content pack contains configuration for Windows 10 Security Events, for Windows Server 2022 Security Event, For Active Directory, For Windows DNS & DHCP Server, for DFS Server.

**Note this was built using filebeat  as the log exporter. No inputs extractor were used, only pipeline rules.**

**Do not need additionnal Grok pattern, uses the default like WORD/GREEDYDATA etc..**

## Includes

* Input (Beats/TCP/5044)
* Stream (Filebeat & Winlogbeat)
* Pipeline Rules w/  stages 
* Lookup table + Data adapter + data cache
* Dashboards 

## Not included

- [macaddress.csv](macaddress_list.csv)
- [dhcpv4_opcode.csv](dhcpv4_opcode.csv)
- 

Add it to your Graylog server in /srv or if different location, modify the content_pack.json to change location path.

## Mandatory Requirements
* Graylog 5.2.0 




## Screenshots



