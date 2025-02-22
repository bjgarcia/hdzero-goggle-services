# HDZGOGGLE Web UI and API
A Web UI and Web API that leverages SumolX's work on services for the HDZero goggles.  

## Services
The BusyBox Web Server only supports Get and Post methods. Other methods, when needed are represented with the URI. Server errors return as primary in the reponse header. Post methods return API errors as misscellaneous in the header. Errors and messages are returned as plain text in the body post response. 

## Settings Api
The first section of the settings file must not be changed. The goggle will reset the entire file if it is missing. Changes to the Settings file are loaded on the next goggle reboot.

### Get Methods
Get a line from a section by name or all lines from a section by section name or get all sections.

```
/cgi-bin/settings?section=wifi&line=ap_ssid
/cgi-bin/settings?section=wifi
/cgi-bin/settings
```
EXAMPLE:{"section":"wifi","setting_list":[{"key": "clientid", "value": "ABC123", "line": "14"},{"key": "enable", "value": "false", "line": "15"},{"key": "mode", "value": "1", "line": "16"},{"key": "ap_ssid", "value": "HDZero", "line": "17"},{"key": "ap_passwd", "value": "divimath", "line": "18"},{"key": "sta_ssid", "value": "SSID", "line": "19"},{"key": "sta_passwd", "value": "abc123", "line": "20"},{"key": "dhcp", "value": "true", "line": "21"},{"key": "ip_addr", "value": "192.168.1.122", "line": "22"},{"key": "netmask", "value": "255.255.255.0", "line": "23"},{"key": "gateway", "value": "192.168.1.1", "line": "24"},{"key": "dns", "value": "192.168.1.1", "line": "25"},{"key": "rf_channel", "value": "6", "line": "26"},{"key": "root_pw", "value": "divimath", "line": "27"},{"key": "ssh", "value": "true", "line": "28"}]}

### Post Methods
Create new lines or a new section after the line or section provided in the URI, at the end of the section, or at the end of file if no line or section is provided in the URI. If lines or section exists an error will be returned. Lines that don't exits will be created in an existing section.

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
Delete one or more lines in a section? Throws error if a line does not exist.

```
/cgi-bin/settings?delete=line
```

## File Api
API should operate on the SD Card mount point only. Anything outside of that should be prevented. Prevent malicious code by scrubbing paths and limiting file names. Keep it simple.

### Get Methods
Only accepts simple globs with a single period. Example filter="filename*.jpg", filter = "*.jpg", or filter="filename.*" will work.  

```
/cgi-bin/file?list=path[&filter=jpg]
/cgi-bin/file?download=path
/cgi-bin/file?stat=path
```
EXAMPLE:
stat:{"path":"/mnt/extsd/movies","used":10987,"available":4096}
list:{ "info_list":[  {"name":"/mnt/extsd/movies/hdz_000.jpg","date":"2024-12-27", "time": "09:07:28.927907079", "offset": "-0500", "size": "6976", "permissions": "-rw-r--r--"}, {"name":"/mnt/extsd/movies/hdz_001.jpg","date":"2024-12-27", "time": "09:07:29.010907083", "offset": "-0500", "size": "6976", "permissions": "-rw-r--r--"}, {"name":"/mnt/extsd/movies/hdz_002.jpg","date":"2024-12-27", "time": "09:07:29.443907100", "offset": "-0500", "size": "11072", "permissions": "-rw-r--r--"}, {"name":"/mnt/extsd/movies/hdz_004.jpg","date":"2024-12-27", "time": "09:07:29.535907104", "offset": "-0500", "size": "6976", "permissions": "-rw-r--r--"}, {"name":"/mnt/extsd/movies/hdz_006.jpg","date":"2024-12-27", "time": "09:07:29.705907111", "offset": "-0500", "size": "6976", "permissions": "-rw-r--r--"} ] }

### Post Methods
Upload a new file. MultiPart/Form file limited to just under 25mg (24400 x 1024).

```
/cgi-bin/file?post=upload
```

### Put Methods
Copy one or more files. Takes an array of source and destination paths.

```
/cgi-bin/file?put=file
```
{"fr_to_list":[{"from_path":"/mnt/extsd/movies/save_this.jpg","to_path":"/mnt/extsd/movies/stars/save_this.jpg"},{"from_path":"/mnt/extsd/movies/stars/save_this.ts","to_path":"/mnt/extsd/movies/save_this.ts"}]}

### Patch Methods
Move or rename one or more files. Takes an array of source and destination paths. 

```
/cgi-bin/file?patch=file
/cgi-bin/file?patch=format
```
{"fr_to_list":[{"from_path":"/mnt/extsd/movies/save_this.jpg","to_path":"/mnt/extsd/movies/stars/save_this.jpg"},{"from_path":"/mnt/extsd/movies/stars/save_this.ts","to_path":"/mnt/extsd/movies/save_this.ts"}]}

### Delete Methods
Delete all files from a list of directories or delete a list of files. Displays the number of lines deleted if directory. Displays each path delete or error if does not exist.

```
/cgi-bin/file?delete=file
/cgi-bin/file?delete=directory
```

## Donation
If you appreciate this work or find the functionality useful. Please consider donating to support this project.

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.me/WillWorks341)
