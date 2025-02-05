# HDZGOGGLE Web Api
A Web API that leverages SumolX's work on services for the HDZero goggles.  

## Services
```
WebUI
Settings API
SD Card API 
```
## Settings Api

Methods return the effected section and effected lines of settings with current line numbers. Line numbers are not part of the file. They are just included for clarity. The first section of the settings file must not be changed. The goggle will reset the entire file if it is missing. The busybox web server only supports Get and Post methods, the other methods are represented in the URI. Only server errors return as primary in the reponse header, api errors are return as misscellaneous in the header and body of the response.

## Get Methods

```
Get a line from a section by name or all lines from a section by name or all sections.
/cgi-bin/settings?section=wifi&line=ap_ssid
/cgi-bin/settings?section=wifi
/cgi-bin/settings
```
EXAMPLE:{"section":"wifi","setting":[{"key": "clientid", "value": "ABC123", "line": "14"},{"key": "enable", "value": "false", "line": "15"},{"key": "mode", "value": "1", "line": "16"},{"key": "ap_ssid", "value": "HDZero", "line": "17"},{"key": "ap_passwd", "value": "divimath", "line": "18"},{"key": "sta_ssid", "value": "SSID", "line": "19"},{"key": "sta_passwd", "value": "abc123", "line": "20"},{"key": "dhcp", "value": "true", "line": "21"},{"key": "ip_addr", "value": "192.168.1.122", "line": "22"},{"key": "netmask", "value": "255.255.255.0", "line": "23"},{"key": "gateway", "value": "192.168.1.1", "line": "24"},{"key": "dns", "value": "192.168.1.1", "line": "25"},{"key": "rf_channel", "value": "6", "line": "26"},{"key": "root_pw", "value": "divimath", "line": "27"},{"key": "ssh", "value": "true", "line": "28"}]}

## Post Methods

Create a new settings line or settings section after the line or section provided in the URI or at the end of the section or file if no line or section is provided in the URI. Lines that already exist in the section will not be changed and an error will be returned indicating line already exist.

```
/cgi-bin/settings?post=line[&line=month]
/cgi-bin/settings?post=section[&section=clock]
```
## Put Methods

Replaces all lines in a section. Return an error if the section does not exist. A section PUT will replace the section provided in the URI.

```
/cgi-bin/settings?put=line/
/cgi-/cgi-bin/settings?put=section&section=section
```
## Patch Methods

Change one or more line values in a section. Throw error if line or section does not exist.

```
/cgi-bin/settings?patch=line
```
## Delete Methods

Delete one or more lines in a section. Returns the deleted lines. Throw error if does not exist.

```
/cgi-bin/settings?delete=line
```

# Donation
If you enjoyed this work or would like to see additional features and functionality added in the future please feel free to donate.

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.me/WillWorks341)
