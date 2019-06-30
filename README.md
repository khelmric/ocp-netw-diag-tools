# OpenShift Network Diagnostic Tools

## Included tools
### Simple network diagnostic tools
#### Ping (`ping`)
#### Tracepath (`tracepath`)
#### My traceroute (`mtr`)
* MTR combines the functions of the traceroute and ping programs in one network diagnostic tool.
#### NetCat (`nc`)
    + Ncat is a feature packed networking utility which will read and
write data across a network from the command line. It uses both
TCP and UDP for communication and is designed to be a reliable
back-end tool to instantly provide network connectivity to other
applications and users. Ncat will not only work with IPv4 and IPv6
but provides the user with a virtually limitless number of potential
uses.
#### iPerf/iPerf3 (`iperf` & `iperf3`)
* iPerf is a widely used tool for network performance measurement and tuning. It is significant as a cross-platform tool that can produce standardized performance measurements for any network. Iperf has client and server functionality, and can create data streams to measure the throughput between the two ends in one or both directions. Typical Iperf output contains a time-stamped report of the amount of data transferred and the throughput measured. 
* iPerf3 is a tool for active measurements of the maximum achievable bandwidth on IP networks. It supports tuning of various parameters related to timing, buffers and protocols (TCP, UDP, SCTP with IPv4 and IPv6). For each test it reports the bandwidth, loss, and other parameters.

### Network loadtest tools
#### ApacheBench (`ab`)
* AB is a single-threaded command line computer program for measuring the performance of HTTP web servers. It is generic enough to test any web server. 
* Example:
```
$ oc rsh diag-tools-7-66p72

sh-4.2$ ab -n 500 -c 50 http://httpd-example-testroute.example.net:80/
This is ApacheBench, Version 2.3 <$Revision: 1430300 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking httpd-example-testroute.example.net (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Finished 500 requests


Server Software:        Apache/2.4.34
Server Hostname:        httpd-example-testroute.example.net
Server Port:            80

Document Path:          /
Document Length:        1024 bytes

Concurrency Level:      50
Time taken for tests:   0.093 seconds
Complete requests:      500
Failed requests:        0
Write errors:           0
Total transferred:      715000 bytes
HTML transferred:       512000 bytes
Requests per second:    5364.69 [#/sec] (mean)
Time per request:       9.320 [ms] (mean)
Time per request:       0.186 [ms] (mean, across all concurrent requests)
Transfer rate:          7491.71 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        1    3   1.0      3       6
Processing:     2    5   1.9      5      12
Waiting:        2    4   1.7      4      10
Total:          4    8   2.0      8      15

Percentage of the requests served within a certain time (ms)
  50%      8
  66%      9
  75%     10
  80%     10
  90%     11
  95%     12
  98%     13
  99%     13
 100%     15 (longest request)
sh-4.2$
```

#### Multiple-host HTTP(s) Benchmarking tool (`mb`)
* The mb client aims to be a clean, simple and scalable tool to generate significant HTTP(s) load against multiple targets from a single host. It also has a per-target reporting functionality. It combines a multithreaded design with scalable event notification systems.
    + source: https://github.com/jmencak/mb/
* Example:
```
$ oc rsh diag-tools-7-66p72

sh-4.2$ cat /tmp/httpd-example.json
[
  {
    "scheme": "http",
    "host": "httpd-example-testroute.example.net",
    "port": 80,
    "method": "GET",
    "path": "/",
    "max-requests": 100000,
    "delay": {
      "min": 10,
      "max": 20,
    },
    "keep-alive-requests": 10,
    "clients": 500,
  }
]

sh-4.2$
sh-4.2$ /loadtest/mb/mb -i /tmp/httpd-example.json -t 50 -d 10
Time: 10.02s
Sent: 5.35MiB, 546.29kiB/s
Recv: 55.95MiB, 5.58MiB/s
Hits: 41888, 4179.37/s
```

## Deployment on OpenShift
```
oc new-app https://github.com/khelmric/ocp-netw-diag-tools.git [--build-env HTTPS_PROXY=httpd://<PROXY_USER>:<PROXY_PASSWORD>@<PROXY_IP>:<PROXY_PORT> --build-env HTTP_PROXY=http://<PROXY_USER>:<PROXY_PASSWORD>@<PROXY_IP>:<PROXY_PORT> -e HTTPS_PROXY=httpd://<PROXY_USER>:<PROXY_PASSWORD>@<PROXY_IP>:<PROXY_PORT> -e HTTP_PROXY=http://<PROXY_USER>:<PROXY_PASSWORD>@<PROXY_IP>:<PROXY_PORT>]
```
