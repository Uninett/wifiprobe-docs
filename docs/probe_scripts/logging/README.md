## Logging
Information like errors, what scripts have been called, and whether the sent data
was successfully received, is logged to the file control_program.log. At the
moment it is just saved locally, and not sent to any external server.

A typical log output may look like this:

```
2016-08-19 08:14:39,055 INFO | Initializing script and io manager
2016-08-19 08:14:39,059 INFO | Connecting wlan0 to the internet
2016-08-19 08:14:39,060 INFO | Calling: ['/root/scripts/probefiles/connect_8812.sh', 'any']
2016-08-19 08:15:13,069 INFO | Sending results to influxdb
2016-08-19 08:15:13,129 INFO | Results successfully received by influxdb.
```
