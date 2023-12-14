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

You need to download manually the CSV. 

- [macaddress.csv](csv/macaddress_list.csv)
- [dhcpv4_opcode.csv](csv/dhcpv4_opcode.csv)
- [file_monitoring_permissions.csv](csv/file_monitoring_permissions.csv)
- [registry.csv](csv/registry.csv)
- [windows_id.csv](csv/windows_id.csv)
- [Windows-EventID-to-EventDescription.csv](csv/Windows-EventID-to-EventDescription.csv)

Add it to your Graylog server in /srv.
If different location, modify the content_pack.json to change location path (CTRL + F and replace all occurences with the desired path)

If you do not add it, some Dashboards will not display all infos, these CSV are used for Lookup Table to enrich data.



## Mandatory Requirements
* Graylog 5.2.0 


## Agents Configuration



## Screenshots



