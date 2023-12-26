# search Nginx log
- Search log nginx Geolocation access log
## config log nginx and push to Splunk
```sh
## config log format nginx in http block
log_format combined1 '$remote_addr - $remote_user [$time_local] '
                    '"$request" $status $body_bytes_sent '
                    '"$http_referer" "$http_user_agent"';
## push log to splunk in Server block
access_log syslog:server=103.121.88.13:514,facility=local7,tag=nginx,severity=info combined;
```
```sh
# search in Plunk with geoip and regular expression, table
index=* sourcetype=syslog host="103.83.212.5" | rex "nginx: (?<client_ip>\d+\.\d+\.\d+\.\d+) - - \[(?<timestamp>[^\]]+)\] \"(?<http_method>[A-Z]+) (?<requested_url>[^\"]+)" | where NOT client_ip="103.83.212.19"| iplocation client_ip | table timestamp, client_ip, http_method, requested_url, City, Country
```
