# iperf3
Network performance testing.

## server
Listen on TCP port 5201 (by default).
```QUIZ: Open port for client's iperf connections```

```bash
# -s: server mode 
# server: 172.22.14.83
# client: 172.22.14.73

$ iperf3 -s -i 10

-----------------------------------------------------------
Server listening on 5201
-----------------------------------------------------------
Accepted connection from 172.22.14.73, port 42212
[  5] local 172.22.14.83 port 5201 connected to 172.22.14.73 port 42214
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-10.00  sec   454 MBytes   381 Mbits/sec                  
[  5]  10.00-20.00  sec   454 MBytes   381 Mbits/sec                  
[  5]  20.00-30.00  sec   482 MBytes   404 Mbits/sec                  
[  5]  30.00-40.00  sec   479 MBytes   402 Mbits/sec                  
[  5]  40.00-50.00  sec   449 MBytes   376 Mbits/sec                  
[  5]  50.00-60.00  sec   461 MBytes   387 Mbits/sec                  
[  5]  60.00-60.04  sec  1.40 MBytes   335 Mbits/sec                  
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth
[  5]   0.00-60.04  sec  0.00 Bytes  0.00 bits/sec                  sender
[  5]   0.00-60.04  sec  2.71 GBytes   388 Mbits/sec                  receiver
```

## client
```bash
# -c: client mode
# -i: interval (seconds) between bandwidth reports
# -t: time in seconds to transmit
# -w: windows size  / socket buffer size
# server: 172.22.14.83
# client: 172.22.14.73

$ iperf3 -i 10 -w 10K -t 60 -c <server IP>

Connecting to host 172.22.14.83, port 5201
[  4] local 172.22.14.73 port 42214 connected to 172.22.14.83 port 5201
[ ID] Interval           Transfer     Bandwidth       Retr  Cwnd
[  4]   0.00-10.00  sec   455 MBytes   382 Mbits/sec    0   17.0 KBytes       
[  4]  10.00-20.00  sec   455 MBytes   381 Mbits/sec    0   26.9 KBytes       
[  4]  20.00-30.00  sec   482 MBytes   404 Mbits/sec    0   26.9 KBytes       
[  4]  30.00-40.00  sec   479 MBytes   402 Mbits/sec    0   26.9 KBytes       
[  4]  40.00-50.00  sec   449 MBytes   376 Mbits/sec    0   26.9 KBytes       
[  4]  50.00-60.00  sec   460 MBytes   386 Mbits/sec    0   26.9 KBytes       
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth       Retr
[  4]   0.00-60.00  sec  2.71 GBytes   389 Mbits/sec    0             sender
[  4]   0.00-60.00  sec  2.71 GBytes   389 Mbits/sec                  receiver
```
