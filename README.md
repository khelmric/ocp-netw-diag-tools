# OpenShift Network Diagnostic Tools

## Included tools
#### Simple network diagnostic tools
* Ping (`ping`)
* Tracepath (`tracepath`)
* My traceroute (`mtr`)
    + MTR combines the functions of the traceroute and ping programs in one network diagnostic tool.
* NetCat (`nc`)
    + Ncat is a feature packed networking utility which will read and
write data across a network from the command line. It uses both
TCP and UDP for communication and is designed to be a reliable
back-end tool to instantly provide network connectivity to other
applications and users. Ncat will not only work with IPv4 and IPv6
but provides the user with a virtually limitless number of potential
uses.
* iPerf/iPerf3 (`iperf` & `iperf3`)
    + iPerf is a widely used tool for network performance measurement and tuning. It is significant as a cross-platform tool that can produce standardized performance measurements for any network. Iperf has client and server functionality, and can create data streams to measure the throughput between the two ends in one or both directions. Typical Iperf output contains a time-stamped report of the amount of data transferred and the throughput measured. 
    + iPerf3 is a tool for active measurements of the maximum achievable bandwidth on IP networks. It supports tuning of various parameters related to timing, buffers and protocols (TCP, UDP, SCTP with IPv4 and IPv6). For each test it reports the bandwidth, loss, and other parameters.

#### Network loadtest tools
* ApacheBench (`ab`)
    + AB is a single-threaded command line computer program for measuring the performance of HTTP web servers. It is generic enough to test any web server. 
* Multiple-host HTTP(s) Benchmarking tool (`mb`)
    + The mb client aims to be a clean, simple and scalable tool to generate significant HTTP(s) load against multiple targets from a single host. It also has a per-target reporting functionality. It combines a multithreaded design with scalable event notification systems.
    + source: https://github.com/jmencak/mb/

## Deployment on OpenShift
```
oc new-app https://github.com/khelmric/ocp-netw-diag-tools.git [--build-env HTTPS_PROXY=httpd://<PROXY_USER>:<PROXY_PASSWORD>@<PROXY_IP>:<PROXY_PORT> --build-env HTTP_PROXY=http://<PROXY_USER>:<PROXY_PASSWORD>@<PROXY_IP>:<PROXY_PORT> -e HTTPS_PROXY=httpd://<PROXY_USER>:<PROXY_PASSWORD>@<PROXY_IP>:<PROXY_PORT> -e HTTP_PROXY=http://<PROXY_USER>:<PROXY_PASSWORD>@<PROXY_IP>:<PROXY_PORT>]
```
