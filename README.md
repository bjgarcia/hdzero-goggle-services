# HDZGOGGLE Web Api
A Web API that leverages SumolX's work on services for the HDZero goggles.  

## Services
```
WebUi and Web services for changing goggle settings and manipulating files. 
```
## Settings Api

Methods return current line numbers but be careful line numbers are not part of the file. They are just included for clarity. The first section of the settings file must not be changed. The goggle will reset the entire file if it is missing. Calls return a section of the settings with pertinent lines and identifing line numbers. The busybox web server only supports Get and Post methods, the other methods are represented in the URI. Server are returned but only server errors return as primary in the header.

## Get Methods

```
Get a line from a section by name or all lines from a section by name or all sections.
/cgi-bin/settings?section=wifi&line=ap_ssid
/cgi-bin/settings?section=wifi
/cgi-bin/settings
```

EXAMPLE:{"section":"wifi","setting":[{"key": "clientid", "value": "ABC123", "line": "14"},{"key": "enable", "value": "false", "line": "15"},{"key": "mode", "value": "1", "line": "16"},{"key": "ap_ssid", "value": "HDZero", "line": "17"},{"key": "ap_passwd", "value": "divimath", "line": "18"},{"key": "sta_ssid", "value": "SSID", "line": "19"},{"key": "sta_passwd", "value": "abc123", "line": "20"},{"key": "dhcp", "value": "true", "line": "21"},{"key": "ip_addr", "value": "192.168.1.122", "line": "22"},{"key": "netmask", "value": "255.255.255.0", "line": "23"},{"key": "gateway", "value": "192.168.1.1", "line": "24"},{"key": "dns", "value": "192.168.1.1", "line": "25"},{"key": "rf_channel", "value": "6", "line": "26"},{"key": "root_pw", "value": "divimath", "line": "27"},{"key": "ssh", "value": "true", "line": "28"}]}

## Post Methods

Create a new settings line or settings section after the line provided in the URI or at the end of the section. Lines that already exist in the section will not be changed and an error will be returned. Return a new section or existing section with new lines.

```
/cgi-bin/settings?post=line[&line=month]
/cgi-bin/settings?post=section[&section=clock]
```


# Donation
If you enjoyed this work or would like to see additional features and functionality added in the future please feel free to donate.

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.me/WillWorks341)
