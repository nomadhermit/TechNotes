## Background
OpenObserve deprecated their native Syslog listener.  As such devices sending syslog messages no longer were able to send data.

## Goal
Minimize changes to existing systems sending syslog messages such as devices with /etc/rsyslog.conf setup with 
```
## syslog server
*.* @<syslog server>:5514
```
in this case, port 5514 was the defaulr syslog listerner port in prior versions of OpenObserve before being deprecated

## Solution
1. Installed Fluent-Bit on same server as OpenObserve
2. Configured /etc/fluent-bit/fluent-bit.conf with the following input and output to proxy syslog message using old syslog listener port to OpenObserve http api listener:
```
[INPUT]
    Name        syslog
    Mode        udp
    Listen      0.0.0.0
    Port        5514
    Parser      syslog-rfc3164
    Tag         syslog-data

[OUTPUT]
    Name http
    Match *
    URI /api/default/default/_json
    Host <OpenObserve server> 
    Port 5080
    tls Off
    Format json
    Json_date_key    time
    HTTP_User <userid> 
    HTTP_Passwd <password> 
    compress gzip
```
3. restart Fluent-bit
4. confirm messages appear in OpenObserve

## Notes:
OpenObserver recommended Fluent-bit setup is to use "Json_date_key" equal to "_Timestamp"  but during testing, 
issues with proxied messages being to old to process showed up in Fluentbit logs so changed to value "time"
and proxied messages went through successfully and had corret timestamp in OpenObserve

During initial troubleshooting used logger to test communication and proxy using following syntax:
```
logger -p 0 -d -n <Fluent-Bit host> -P 5514 "This is only test message -- #1 --- remote"
```
