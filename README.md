# OpenShift Network Diagnostic Tools

## Included tools
### Simple network diagnostic tools
#### Ping (`ping`)
* Example:
```
$ oc rsh diag-tools-7-66p72

sh-4.2$ ping -c 4 httpd-example-testroute.example.net
PING httpd-example-testroute.example.net (160.48.88.115) 56(84) bytes of data.
64 bytes from dispatch-cnap-00-mp-staging2.bmwgroup.net (160.48.88.115): icmp_seq=1 ttl=252 time=0.747 ms
64 bytes from dispatch-cnap-00-mp-staging2.bmwgroup.net (160.48.88.115): icmp_seq=2 ttl=252 time=1.07 ms
64 bytes from dispatch-cnap-00-mp-staging2.bmwgroup.net (160.48.88.115): icmp_seq=3 ttl=252 time=1.45 ms
64 bytes from dispatch-cnap-00-mp-staging2.bmwgroup.net (160.48.88.115): icmp_seq=4 ttl=252 time=1.29 ms

--- httpd-example-testroute.example.net ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 0.747/1.143/1.453/0.265 ms
sh-4.2$

```

#### Tracepath (`tracepath`)
* Example:
```
oc rsh diag-tools-7-66p72

sh-4.2$ tracepath 10.221.4.131
 1?: [LOCALHOST]                                         pmtu 1450
 1:  10.221.4.131                                          2.555ms reached
 1:  10.221.4.131                                          1.018ms reached
     Resume: pmtu 1450 hops 1 back 1
sh-4.2$

```

#### My traceroute (`mtr`)
* MTR combines the functions of the traceroute and ping programs in one network diagnostic tool.
```
oc rsh diag-tools-7-66p72

sh-4.2$ mtr httpd-example-testroute.example.net

                                                                My traceroute  [v0.85]
diag-tools-7-66p72 (0.0.0.0)                                                                                                 Sun Jun 30 18:22:47 2019
Keys:  Help   Display mode   Restart statistics   Order of fields   quit
                                                                                                             Packets               Pings
 Host                                                                                                      Loss%   Snt   Last   Avg  Best  Wrst StDev
 1. 10.221.0.1                                                                                              0.0%     4    0.1   0.2   0.1   0.4   0.0
 2. example-self-m.net                                                                                   0.0%     4    1.0   0.9   0.8   1.1   0.0
 3. 192.168.11.50                                                                                           0.0%     4    1.9   1.7   1.4   1.9   0.0
 4. dispatch-example.net                                                                               0.0%     4    1.4   1.3   1.1   1.5   0.0

```

#### NetCat (`nc`)
* Ncat is a feature packed networking utility which will read and
write data across a network from the command line. It uses both
TCP and UDP for communication and is designed to be a reliable
back-end tool to instantly provide network connectivity to other
applications and users. Ncat will not only work with IPv4 and IPv6
but provides the user with a virtually limitless number of potential
uses.
* Example (listening on port 1234):
```
$ oc rsh diag-tools-7-66p72

sh-4.2$ nc -l 1234
```

#### iPerf/iPerf3 (`iperf` & `iperf3`)
* iPerf is a widely used tool for network performance measurement and tuning. It is significant as a cross-platform tool that can produce standardized performance measurements for any network. Iperf has client and server functionality, and can create data streams to measure the throughput between the two ends in one or both directions. Typical Iperf output contains a time-stamped report of the amount of data transferred and the throughput measured. 
* Example:
```
oc rsh diag-tools-7-66p72

sh-4.2$ iperf -c 10.221.5.141 -p 8080 -f -M --parallel 5
------------------------------------------------------------
Client connecting to 10.221.5.141, TCP port 8080
TCP window size:  848 Kbit (default)
------------------------------------------------------------
[  7] local 10.221.0.111 port 41844 connected with 10.221.5.141 port 8080
[  4] local 10.221.0.111 port 41838 connected with 10.221.5.141 port 8080
[  5] local 10.221.0.111 port 41840 connected with 10.221.5.141 port 8080
[  3] local 10.221.0.111 port 41836 connected with 10.221.5.141 port 8080
[  6] local 10.221.0.111 port 41842 connected with 10.221.5.141 port 8080
[ ID] Interval       Transfer     Bandwidth
[  7]  0.0-10.0 sec  2.91 Gbits   291 Mbits/sec
[  4]  0.0-10.0 sec  4.96 Gbits   496 Mbits/sec
[  5]  0.0-10.0 sec  7.22 Gbits   722 Mbits/sec
[  3]  0.0-10.0 sec  4.70 Gbits   470 Mbits/sec
[  6]  0.0-10.0 sec  5.69 Gbits   568 Mbits/sec
[SUM]  0.0-10.0 sec  25.5 Gbits  2.54 Gbits/sec
sh-4.2$
```
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
