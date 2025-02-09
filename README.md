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
* Graylog 6.2.X
* Winlogbeat & Filebeat 7.17.27 (Latest version at the time of writing)
* Edit Windows-Security-Content-Pack.json before uploading it ! (See requirements)


## Agents Configuration (Requirement)

I'm still using the 7.X version of Beats agents as there is some change since the 8 versions (like modules not loaded anymore by the agents)

Winlogbeat Security & Powershell Module are included in non-oss Beats archive.


- Filebeat
[Download filebeat archive and extract .exe](https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.17.27-windows-x86_64.zip)
Extract the archive and add it to your server/workstation: `C:\Program Files (x86)\filebeat\7.17.27`, replace the filebeat.yml with this one:  LINK OF FILEBEAT.YML

- Winlogbeat
[Download winlogbeat archive and extract .exe](https://artifacts.elastic.co/downloads/beats/winlogbeat/winlogbeat-7.17.27-windows-x86_64.zip)

Extract the archive and add it to your server/workstation: `C:\Program Files (x86)\winlogbeat\7.17.27`, replace the winlogbeat.yml with this one:  LINK OF WINLOGBEAT.YML




## Edit Windows-Security-Content-Pack.json (Requirement)

I've made some Dashboard based on Server names to filter in or out some event logged. You will need to adjust the filter based on your infrastructure.

- Follow these instructions:

  - Search & replace (use Notepadd for example):
  
    - `srv*`  ---> this filter means all Netbios name starting with srv (eg: srvdfs, srvad1, etc), I use it to show only computers data on dashboard by using NOT conditions, you should replace this filter with either the name of all of your servers or another field key which is easier to implement and that identify all servers.
      - replace the string `srv*` by `(name1 OR name2 OR name3)` where nameX is all your servers name
      - <img width="600" alt="image" src="https://github.com/s0p4L1n3/Graylog_Content_Pack_Windows_Security/assets/126569468/d4456e70-11a0-431d-a700-c43a8ea39994">
    - `(srvad1 OR srvad2)` --> on my test prod, I have 2 AD DC, I use a filter where I want to show data only from my 2 DC
      - replace the strings `(srvad1 OR srvad2)` by `(DCname1 OR DCname2 OR DCname3)` where DCnameX is all your DC name

    - `srvdfs1` --> on my test prod, I have a DFS Server hosting SMB Share, so I created a Dashboard to monitor files event for this server, if you don't have one you can ignore and delete the dashboard tab on the Web UI.
       - replace the string `srvdfs1` by `yourdfsname` if you have one
    - `Europe/Paris` --> on my test prod, I'm in France so the Timezone is this one, if you are from another timezone, replace with the desired one
       - replace the string `Europe/Paris` by `Country/Town` timezone of your choice
    - `graylog.lab.lan` --> it is my test domain FQDN, change it according to your server FQDN / IP Address, so that all sidecars are correctly configured to send data to your Graylog Server
       - replace the string `graylog.lab.lan` by `graylog.your.fqdn.com` which normally should correspond to the FQDN point to your graylog server
      - ![image](https://github.com/s0p4L1n3/Graylog_Content_Pack_Windows_Security/assets/126569468/b35ee04f-d87b-4a01-8499-841e36ea825e)
    

## Create Index for each stream

By default, the Content Pack can't embeed Index, I recommand you to create one in order to separate Filebeat and Winlogbeat and so on.
I don't think you want to have all data in the same index. It is like eating all the meal ingredient at the same time, it's difficult to recognize the taste of each.

<img width="1292" alt="image" src="https://github.com/s0p4L1n3/Graylog_Content_Pack_Windows_Security/assets/126569468/269618e7-28aa-432f-bc5e-cdd7131db466">

And change the Index for the Winlogbeat stream.

![index_winlogbeat](https://github.com/s0p4L1n3/Graylog_Content_Pack_Windows_Security/assets/126569468/53f01354-7c5d-44d4-9dca-ed12fb5add85)

Repeat the process for Filebeat.

## Screenshots

- Active Directory

<img width="1911" alt="image" src="https://github.com/s0p4L1n3/Graylog_Content_Pack_Windows_Security/assets/126569468/e33125d7-b73d-47ac-8d8e-667f6cc80b53">

- Account Management

<img width="1908" alt="image" src="https://github.com/s0p4L1n3/Graylog_Content_Pack_Windows_Security/assets/126569468/97149acf-e241-4510-9972-d7621d57cbf3">

- Auth

![image](https://github.com/s0p4L1n3/Graylog_Content_Pack_Windows_Security/assets/126569468/f1cc7f62-3120-4130-b3bc-aa7aaa9d1eb9)

- Defender

![image](https://github.com/s0p4L1n3/Graylog_Content_Pack_Windows_Security/assets/126569468/7e96c228-458b-4f4a-9792-612a2b2d1169)

- DHCP Server

![image](https://github.com/s0p4L1n3/Graylog_Content_Pack_Windows_Security/assets/126569468/ff1660a5-c143-4570-8412-812e43331a34)

- DNS Client

![image](https://github.com/s0p4L1n3/Graylog_Content_Pack_Windows_Security/assets/126569468/6ad93f79-3a03-4b86-a3a4-99e9a30835c5)

- DNS Server

![image](https://github.com/s0p4L1n3/Graylog_Content_Pack_Windows_Security/assets/126569468/72c3c518-7aac-4255-b10c-6e586661e540)

- Log Event Viewer

![image](https://github.com/s0p4L1n3/Graylog_Content_Pack_Windows_Security/assets/126569468/e8bb727d-ee6d-477c-bb78-5aa69a0ab524)

- Windows Firewall
- 
![image](https://github.com/s0p4L1n3/Graylog_Content_Pack_Windows_Security/assets/126569468/2778e7db-6284-4d34-996a-2772eaff126e)

![image](https://github.com/s0p4L1n3/Graylog_Content_Pack_Windows_Security/assets/126569468/7287d8a0-8c14-42c9-bd9c-cd6267f10e72)

And so on...

## References

- [jhochwald winlogbeat security](https://github.com/jhochwald/Universal-Winlogbeat-configuration/blob/main/assets/winlogbeat.yml)
- Windows Security Monitoring - Scenarios and Patterns - Book by Andrei Miroshnikov (Very good book, I recommend you to buy it)

