# HDZGOGGLE Web UI and API
A Web UI and Web API that leverages SumolX's work on services for the HDZero goggles.  

## Services
The BusyBox Web Server only supports Get and Post methods. Other methods when needed are represented with the URI. Post methods return API errors as misscellaneous in the header and body of the response. Server errors return as primary in the reponse header.

## Settings Api
The first section of the settings file must not be changed. The goggle will reset the entire file if it is missing.

### Get Methods

```
Get a line from a section by name or all lines from a section by name or all sections.
/cgi-bin/settings?section=wifi&line=ap_ssid
/cgi-bin/settings?section=wifi
/cgi-bin/settings
```
EXAMPLE:{"section":"wifi","setting_list":[{"key": "clientid", "value": "ABC123", "line": "14"},{"key": "enable", "value": "false", "line": "15"},{"key": "mode", "value": "1", "line": "16"},{"key": "ap_ssid", "value": "HDZero", "line": "17"},{"key": "ap_passwd", "value": "divimath", "line": "18"},{"key": "sta_ssid", "value": "SSID", "line": "19"},{"key": "sta_passwd", "value": "abc123", "line": "20"},{"key": "dhcp", "value": "true", "line": "21"},{"key": "ip_addr", "value": "192.168.1.122", "line": "22"},{"key": "netmask", "value": "255.255.255.0", "line": "23"},{"key": "gateway", "value": "192.168.1.1", "line": "24"},{"key": "dns", "value": "192.168.1.1", "line": "25"},{"key": "rf_channel", "value": "6", "line": "26"},{"key": "root_pw", "value": "divimath", "line": "27"},{"key": "ssh", "value": "true", "line": "28"}]}

### Post Methods

Create new lines or a new section after the line or section provided in the URI, at the end of the section, or end of file if no line or section is provided in the URI. If lines or section exists an error will be returned. Lines that don't exits will be created.

```
/cgi-bin/settings?post=line[&line=month]
/cgi-bin/settings?post=section[&section=clock]
```
### Put Methods

Replaces all lines in a section with the lines passed as content. Return an error if the section does not exist.

```
/cgi-bin/settings?put=line
```
### Patch Methods

Change one or more line values in a section. Throw error if line or section does not exist.

```
/cgi-bin/settings?patch=line
```
### Delete Methods

Delete one or more lines in a section. Throws error if a line does not exist.

```
/cgi-bin/settings?delete=line
```

## File Api
Allow directory listing below the mount point only.

### Get Methods

Paths and filters are disassembled, cleaned, and reassembled to prevent funny business. Example filter="filename*.jpg", filter = "*.jpg", or filter="filename.*".  

```
/cgi-bin/file?list=path[&filter=jpg]
```
EXAMPLE:

### Post Methods

Upload a new file 

```
/cgi-bin/settings?post=file
```
### Patch Methods

Rename one or more 

```
/cgi-bin/settings?patch=file
```
### Delete Methods

Delete one or more

```
/cgi-bin/settings?delete=file
/cgi-bin/settings?delete=directory
```



## Donation
If you enjoyed this work or would like to see additional features and functionality added in the future please feel free to donate.

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.me/WillWorks341)
