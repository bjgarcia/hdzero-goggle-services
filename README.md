# HDZGOGGLE Web UI and API
A Web UI and Web API that leverages SumolX's work on services for the HDZero goggles.  

## Services
These services allow applications to access and update the SD card and goggle settings. Goggle settings are changed in the file that is used to persist goggle settings. The changes will be loaded on reboot. The BusyBox Web Server only supports Get and Post methods. Other methods, when needed are represented with the URI. Server errors are returned as primary in the reponse header. API errors are returned as misscellaneous in the header. Errors and messages are returned as plain text in the body of the response. I will implement a login but security is going to be a issue becuase the web server currently does not support HTTPS. Any one with access to your net work could gain access to your goggle settings. Don't give anyone access to your goggle network. 

## Settings Api
The first section of the settings file must not be changed. The goggle will reset the entire file if it is missing. Changes to the Settings file are loaded on the next goggle reboot.

### Get Methods
Get a line or lines from a section by name, or get all sections.

```
/cgi-bin/setting?section=birthday&line=year
{"section":"birthday","setting_list":[{"key": "year", "value": "1980", "line": "57"}]}
/cgi-bin/setting?section=birthday
{"section":"birthday","setting_list":[{"key": "year", "value": "1980", "line": "57"},{"key": "month", "value": "10", "line": "58"},{"key": "day", "value": "3", "line": "59"}]}
/cgi-bin/setting
{"sections":[{"section":"settings","settings":[{"key":"file_version", "value":"1", "ln": "2"}],"osd":[{"key":"is_visible", "value":"false", "ln": "4"},{"key":"embpipedded_mode", "value":"0", "ln": "5"},{"key":"orbit", "value":"0", "ln": "6"},{"key":"startup_visibility", "value":"1", "ln": "7"}],"power":[{"key":"cell_count", "value":"3", "ln": "9"},{"key":"warning_type", "value":"2", "ln": "10"},{"key":"power_ana_rx", "value":"1", "ln": "11"},{"key":"calibration_offset", "value":"0", "ln": "12"}],"wifi":[{"key":"clientid", "value":"067342ACC43FFF", "ln": "14"},{"key":"enable", "value":"true", "ln": "15"},{"key":"mode", "value":"1", "ln": "16"},{"key":"ap_ssid", "value":"HDZero", "ln": "17"},{"key":"ap_passwd", "value":"divimath", "ln": "18"},{"key":"sta_ssid", "value":"Guest", "ln": "19"},{"key":"sta_passwd", "value":"XXXXXXX!", "ln": "20"},{"key":"dhcp", "value":"true", "ln": "21"},{"key":"ip_addr", "value":"192.168.4.110", "ln": "22"},{"key":"netmask", "value":"255.255.255.0", "ln": "23"},{"key":"gateway", "value":"192.168.4.10", "ln": "24"},{"key":"dns", "value":"192.168.2.1", "ln": "25"},{"key":"rf_channel", "value":"6", "ln": "26"},{"key":"root_pw", "value":"divimath", "ln": "27"},{"key":"ssh", "value":"true", "ln": "28"}],"autoscan":[{"key":"last_source", "value":"1", "ln": "30"}],"fans":[{"key":"top_speed", "value":"4", "ln": "32"}],"record":[{"key":"mode_manual", "value":"true", "ln": "34"},{"key":"osd", "value":"false", "ln": "35"},{"key":"audio", "value":"false", "ln": "36"},{"key":"format_ts", "value":"true", "ln": "37"}],"clock":[{"key":"year", "value":"2024", "ln": "39"},{"key":"month", "value":"12", "ln": "40"},{"key":"day", "value":"24", "ln": "41"},{"key":"hour", "value":"11", "ln": "42"},{"key":"min", "value":"0", "ln": "43"},{"key":"sec", "value":"0", "ln": "44"},{"key":"format", "value":"0", "ln": "45"}],"scan":[{"key":"channel", "value":"1", "ln": "47"}],"source":[{"key":"analog_format", "value":"0", "ln": "49"}],"image":[{"key":"oled", "value":"8", "ln": "51"},{"key":"brightness", "value":"39", "ln": "52"},{"key":"saturation", "value":"28", "ln": "53"},{"key":"contrast", "value":"25", "ln": "54"},{"key":"auto_off", "value":"1", "ln": "55"}]}]}
```

### Post Methods
Create new lines or a new section after the line or section provided in the URI given. Or create new lines or a new section at the end of the section or the end of file if no line or section is provided in the URI. If lines or section exists a error will be returned. Lines that don't exits will be created in existing section or section and line will be created if both are new. Takes a JSON Section object with one or more Lines. Returns the inserted section.

```
/cgi-bin/setting?post=line
{"section":"birthday","setting_list":[{"key":"year","value":"2024","line":"00"}]}
{"section":"birthday","setting_list":[{"key":"year","value":"2024","line":"57"}]}
```
```
/cgi-bin/setting?post=line&after=month
{"section":"birthday","setting_list":[{"key":"day","value":"24","line":"00"}]}
{"section":"birthday","setting_list":[{"key":"day","value":"24","line":"59"}]}
```
```
/cgi-bin/setting?post=section
{"section":"birthday","setting_list":[{"key":"year","value":"2024","line":"00"},{"key":"month","value":"04","line":"00"},{"key":"day","value":"24","line":"00"}]}
{"section":"birthday","setting":[{"key":"year","value":"2024","line":"57"},{"key":"month","value":"04","line":"58"},{"key":"day","value":"24","line":"59"}]}
```
```
/cgi-bin/setting?post=section&after=clock
{"section":"birthday","setting_list":[{"key":"year","value":"2024","line":"00"},{"key":"month","value":"04","line":"00"},{"key":"day","value":"24","line":"00"}]}
{"section":"birthday","setting":[{"key":"year","value":"2024","line":"47"},{"key":"month","value":"04","line":"48"},{"key":"day","value":"24","line":"49"}]}
```

### Put Methods
Replaces all lines in a section with one or more lines passed as content. Return an error if the section does not exist.

```
/cgi-bin/setting?put=line
{"section":"birthday","setting_list":[{"key":"Year","value":"2025","line":"00"},{"key":"Month","value":"06","line":"00"}]}
{"section":"birthday","setting_list":[{"key":"Year","value":"2025","line":"57"},{"key":"Month","value":"06","line":"58"}]}
```

### Patch Methods
Change one or more line values in a section. Throw error if line or section does not exist.

```
/cgi-bin/setting?patch=line
{"section":"birthday","setting_list":[{"key":"Year","value":"1954","line":"00"},{"key":"Month","value":"02","line":"00"},{"key":"Day","value":"02","line":"00"}]}
{"section":"birthday","setting_list":[{"key":"Year","value":"1954","line":"57"},{"key":"Month","value":"02","line":"58"}]}
Status: 400 Bad Request
Error: line does not exist.
```

### Delete Methods
Delete one or more lines in a section or delete entire section? Throws error if a line or section does not exist. Returns the delected lines and section.

```
/cgi-bin/setting?delete=line
{"section":"birthday","setting_list":[{"key":"year","value":"2024","line":"00"}]}
{"section":"birthday","setting_list":[ {"key": "year", "value": "2024", "line": "57"}, ] }
```
```
/cgi-bin/settings?delete=section
{"section":"birthday","setting_list":[]}
{"section":"birthday","setting_list":[{"key":"year","value":"2024","line":"57"},{"key":"month","value":"05","line":"58"},{"key":"day","value":"24","line":"59"}]}
```

## File Api
API operates on the SD Card mount point only. It discourages malicious code by scrubbing paths and limiting file names. The connection is not secure. Don't give anyone access to your network. Uploads and downloads are limited to 25 MG.

### Get Methods
List will accepts simple globs and a single period extension. Example filter="filename*.jpg", filter = "*.jpg", or filter="filename.*" will work. The stat method will accept a path outside of sd card.

```
/cgi-bin/file
/cgi-bin/file?list=
{"info_list":[ {"name":"FSCK","date":"2025-02-26","time":"18:16:06.618056704","offset":"-0500","size":"4096","permissions":"drwxr-xr-x"},{"name":"isp0_0_0_0_ctx_saved.bin","date":"2025-02-26","time":"18:14:53.551053720","offset":"-0500","size":"40472","permissions":"-rw-r--r--"},{"name":"movies","date":"2025-02-23","time":"13:28:37.747766228","offset":"-0500","size":"4096","permissions":"drwxr-xr-x"}]}
/cgi-bin/file?list=/movies/stars
cgi-bin/file?list=/movies/stars&filter=*
{"info_list":[ {"name":"save_this.jpg","date":"2025-02-13","time":"17:41:29.305099140","offset":"-0500","size":"11072","permissions":"-rw-r--r--"}]}
cgi-bin/file?list=/movies&filter=*.jpg
{"info_list":[ {"name":"/mnt/extsd//movies/hdz_000.jpg","date":"2025-02-13","time":"12:27:03.301328631","offset":"-0500","size":"6976","permissions":"-rw-r--r--"},{"name":"/mnt/extsd//movies/hdz_001.jpg","date":"2025-02-13","time":"12:27:04.232328669","offset":"-0500","size":"6976","permissions":"-rw-r--r--"},{"name":"/mnt/extsd//movies/hdz_004.jpg","date":"2025-02-13","time":"12:27:10.074328908","offset":"-0500","size":"6976","permissions":"-rw-r--r--"},{"name":"/mnt/extsd//movies/hdz_006.jpg","date":"2025-02-13","time":"12:27:11.799328978","offset":"-0500","size":"6976","permissions":"-rw-r--r--"},{"name":"/mnt/extsd//movies/save_this.jpg","date":"2025-02-13","time":"12:27:09.060328867","offset":"-0500","size":"11072","permissions":"-rw-r--r--"}]}
```
```
/cgi-bin/file?stat=/tmp
{"path":"/dev/root","used":"53078148","available":"856419708"}
```
```
/cgi-bin/file?download/mnt/extsd/movies/hdz_006.mp4
Opens the download file dialog to download the file. Size is limited.
```

### Post Methods
Upload expects one or more files submitted in the multipart-form format. Uploads overwrite existing files. The files are parsed and saved by an added c++ executable for performance reasons.

```
/cgi-bin/file?post=upload
```

### Put Methods
Copy one or more files or convert a file to mp4. Takes an array of source and destination paths. Conversion uses ffmpeg with libx264 library built in. There is likley more work that could be done here. Resultng files are about 25% size of the orginal. Conversions overwrite and a .log file is created for each conversion with the output file name. Could add something that checks the log for completed conversion.

```
/cgi-bin/file?put=file
/cgi-bin/file?put=mp4

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
