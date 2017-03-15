## The WiFi scripts
The WiFi testing scripts reside in
ansible-probes/roles/common/files/wifi_scripts/. All scripts write results to a
file called results_report, in the format 'key value', e.g. 'dhcp_time_any
2.35'. The file is emptied each time the results are sent.

The following scripts are available. As default, all of them are enabled:

| Filename          | Description
| :---------------- | :-----------------
| connect_8812.sh   | AP & dhcp connection time
| scan.sh           | Scan for number of cells
| collect.sh        | Measure link quality & bitrate
| check_ipv6.sh     | Check if IPv6 is available
| check_http_v4.sh  | Measure HTTP and DNS request time for IPv4
| check_http_v6.sh  | Measure HTTP and DNS request time for IPv6
| rtt4.sh           | Measure round trip time for IPv4
| rtt6.sh           | Measure round trip time for IPv6
| run_owping4.sh    | Measure one-way connection time for IPv4
| run_owping6.sh    | Measure one-way connection time for IPv6
| run_bwctl4.sh     | Measure throughput for IPv4
| run_bwctl6.sh     | Measure throughput for IPv6
