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
