# Windows Security Content Pack for Graylog

Tested with Winlogbeat & Filebeat 7.12.1.0 / Windows 2022 / Windows 10 / Graylog 5.2.2

The Content Pack should be compatible with all Graylog 5.2.X version. 
This content pack contains configuration for Windows 10 Security Events, for Windows Server 2022 Security Event, For Active Directory, For Windows DNS & DHCP Server, for DFS Server.

**Note this was built using filebeat and Winlogbeat as the log exporter. No inputs extractor were used, only pipeline rules.**

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



## Requirements
* Graylog 5.2.0
* Graylog Sidecar Agent 1.5.0
* Winlogbeat & Filebeat 7.12.1
* Winlogbeat Security & Powershell Module


## Agents Configuration

Be careful, Graylog Sidecar 1.5.0 embedd two bad binary version of Filebeat and Winlogbeat which are 8.9.0, OpenSearch is not compatible ! The latest compatible version is 7.12.1.
Replace the two binary with the 7.12.1:

[Download filebeat archive and extract .exe](https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-oss-7.12.1-windows-x86_64.zip)

[Download winlogbeat archive and extract .exe](https://artifacts.elastic.co/downloads/beats/winlogbeat/winlogbeat-oss-7.12.1-windows-x86_64.zip)


## Create your Graylog Sidecar token API

Follow [this](https://go2docs.graylog.org/5-0/getting_in_log_data/graylog_sidecar.html) Graylog guide if you don't know how.

## Add the Winlogbeat Module to your Sidecar folder agent.

```
C:\Program Files\Graylog\sidecar\module
```
<img width="295" alt="image" src="https://github.com/s0p4L1n3/Graylog_Content_Pack_Windows_Security/assets/126569468/7b845b49-85f5-48a2-a152-2d0c3f5e555e">



## Screenshots



