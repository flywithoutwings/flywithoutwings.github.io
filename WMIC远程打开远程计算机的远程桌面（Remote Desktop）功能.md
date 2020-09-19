```
wmic /node:"[full machine name]" /USER:"[domain]\[username]" /password:xx PATH win32_terminalservicesetting WHERE (__Class!="") CALL SetAllowTSConnections 1
```
