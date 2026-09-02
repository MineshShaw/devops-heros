# Networking Commands Practice

This document contains the execution outputs and explanations for fundamental networking commands as part of the devops-hero practice tasks. 

---

## 1. Ping Command

**Command Executed:**
```bash
ping -c 4 google.com
```

**Output:**
```text
PING google.com (142.251.43.206) 56(84) bytes of data.
64 bytes from bkk03s03-in-f14.1e100.net (142.251.43.206): icmp_seq=1 ttl=117 time=14.7 ms
64 bytes from bkk03s03-in-f14.1e100.net (142.251.43.206): icmp_seq=2 ttl=117 time=15.8 ms
64 bytes from bkk03s03-in-f14.1e100.net (142.251.43.206): icmp_seq=3 ttl=117 time=13.9 ms
64 bytes from bkk03s03-in-f14.1e100.net (142.251.43.206): icmp_seq=4 ttl=117 time=17.3 ms

--- google.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3005ms
rtt min/avg/max/mdev = 13.878/15.421/17.297/1.284 ms
```

**Explanation:**
The `ping` command is a fundamental diagnostic tool used to test the reachability of a host on an IP network. It works by sending ICMP (Internet Control Message Protocol) Echo Request packets to the target and waiting for an ICMP Echo Reply. The output shows whether the packets successfully reached the destination, if any packets were lost (packet loss), and how long it took for the round trip (latency/time).

---

## 2. Curl Command

**Command Executed:**
```bash
curl -I https://example.com
```
*(Using `-I` fetches only the HTTP headers for a cleaner output)*

**Output:**
```text
HTTP/2 200
date: Wed, 02 Sep 2026 08:37:37 GMT
content-type: text/html
server: cloudflare
last-modified: Sun, 30 Aug 2026 04:11:49 GMT
allow: GET, HEAD
accept-ranges: bytes
age: 995
cf-cache-status: HIT
cf-ray: a34b319abf8e2e35-MAA
```

**Explanation:**
`curl` (Client URL) is a command-line tool used for transferring data to or from a server using various protocols (HTTP, HTTPS, FTP, etc.). It is heavily used in DevOps to test REST APIs, download files, or check if a web server is responding correctly. In this output, we successfully received a `200 OK` HTTP status code, confirming the web server is up and serving content.

---

## 3. Traceroute Command

**Command Executed:**
```bash
traceroute google.com
```

**Output:**
```text
traceroute to google.com (142.251.43.206), 30 hops max, 60 byte packets
 1  scaler-190NML3.mshome.net (172.21.208.1)  1.462 ms  1.381 ms  1.349 ms
 2  dns.nfen (192.168.1.1)  4.270 ms  4.236 ms  4.195 ms
 3  static-141.166.194.14-tataidc.co.in (14.194.166.141)  12.467 ms  12.374 ms  12.344 ms
 4  * 115.111.221.61.static-banglore.vsnl.net.in (115.111.221.61)  11.944 ms  11.904 ms
 5  172.28.117.90 (172.28.117.90)  11.856 ms * *
 6  115.112.15.114.static-chennai.vsnl.net.in (115.112.15.114)  12.023 ms  13.353 ms  13.313 ms
 7  * * *
 8  142.251.55.238 (142.251.55.238)  14.444 ms 209.85.142.246 (209.85.142.246)  13.014 ms 216.239.43.172 (216.239.43.172)  12.935 ms
 9  172.253.71.2 (172.253.71.2)  12.470 ms 142.250.233.143 (142.250.233.143)  12.372 ms  12.338 ms
10  142.250.208.231 (142.250.208.231)  12.301 ms 142.250.239.229 (142.250.239.229)  12.289 ms 142.251.51.119 (142.251.51.119)  12.236 ms
11  bkk03s03-in-f14.1e100.net (142.251.43.206)  12.282 ms 142.250.233.145 (142.250.233.145)  13.664 ms  13.677 ms
```

**Explanation:**
While `ping` tells you *if* a server is reachable, `traceroute` tells you *how* your traffic gets there. It maps the exact path (or routing hops) that a packet takes from your local machine to the destination server. It does this by gradually increasing the "Time to Live" (TTL) of packets. This is extremely useful for finding out exactly where a connection is failing or lagging across the internet.

---

## 4. Nslookup Command

**Command Executed:**
```bash
nslookup scaler.com
```

**Output:**
```text
Server:         10.255.255.254
Address:        10.255.255.254#53

Non-authoritative answer:
Name:   scaler.com
Address: 18.67.161.3
Name:   scaler.com
Address: 18.67.161.5
Name:   scaler.com
Address: 18.67.161.61
Name:   scaler.com
Address: 18.67.161.8
```

**Explanation:**
`nslookup` (Name Server Lookup) is a tool used to query the Domain Name System (DNS). It translates human-readable domain names into the IP addresses that computers use to communicate. The output shows the DNS server used to resolve the query and the resulting IP addresses for the requested domain.

---

## 5. Ifconfig Command

**Command Executed:**
```bash
ifconfig
```

**Output:**
```text
docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        inet6 fe80::286f:b5ff:febb:ff50  prefixlen 64  scopeid 0x20<link>
        ether 2a:6f:b5:bb:ff:50  txqueuelen 0  (Ethernet)
        RX packets 17  bytes 1263 (1.2 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 23  bytes 1790 (1.7 KB)
        TX errors 0  dropped 2 overruns 0  carrier 0  collisions 0

eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.21.221.97  netmask 255.255.240.0  broadcast 172.21.223.255
        inet6 fe80::215:5dff:fe27:389d  prefixlen 64  scopeid 0x20<link>
        ether 00:15:5d:27:38:9d  txqueuelen 1000  (Ethernet)
        RX packets 15702  bytes 22043065 (22.0 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 2467  bytes 396454 (396.4 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 543  bytes 55450 (55.4 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 543  bytes 55450 (55.4 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

**Explanation:**
`ifconfig` (Interface Configuration) displays the current network configuration of your system's network interfaces (like Wi-Fi or Ethernet cards). It shows crucial details such as the assigned local IP address, subnet mask, MAC address (`ether`), and statistics on transmitted/received packets. 

---

## 6. Netstat Command

**Command Executed:**
```bash
netstat -tuln
```

**Output:**
```text
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 127.0.0.54:53           0.0.0.0:*               LISTEN
tcp        0      0 10.255.255.254:53       0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN
udp        0      0 127.0.0.54:53           0.0.0.0:*
udp        0      0 127.0.0.53:53           0.0.0.0:*
udp        0      0 10.255.255.254:53       0.0.0.0:*
udp        0      0 127.0.0.1:323           0.0.0.0:*
udp        0      0 127.0.0.1:323           0.0.0.0:*
udp6       0      0 ::1:323                 :::*
udp6       0      0 ::1:323                 :::*                      
```

**Explanation:**
`netstat` (Network Statistics) provides detailed information about active network connections, routing tables, and listening ports. The flags `-tuln` are commonly used to show only listening (l) TCP (t) and UDP (u) ports in numerical (n) form. This is highly useful for verifying if a service (like a web server on port 80 or SSH on port 22) is actively running and accepting connections.

---

## 7. Route Command

**Command Executed:**
```bash
route -n
```

**Output:**
```text
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         172.21.208.1    0.0.0.0         UG    0      0        0 eth0
172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 docker0
172.21.208.0    0.0.0.0         255.255.240.0   U     0      0        0 eth0
```

**Explanation:**
The `route` command allows you to view and manipulate the IP routing table of your operating system. The routing table determines where network traffic is directed based on its destination IP. In the output, the `0.0.0.0` destination represents the "default gateway" (`192.168.1.1`), which means any traffic not destined for the local network is sent to the router to be forwarded to the internet.

---

## 8. Hostname Command

**Command Executed:**
```bash
hostname
```

**Output:**
```text
scaler-190NML3
```

**Explanation:**
The `hostname` command simply displays (or sets) the network name of the current machine. This name is used to identify the device on a local network and is often utilized in logs, command prompts, and internal DNS resolution.

---

## 9. Dig Command

**Command Executed:**
```bash
dig scaler.com +short
```

**Output:**
```text
18.67.161.5
18.67.161.8
18.67.161.61
18.67.161.3
```

**Explanation:**
`dig` (Domain Information Groper) is a powerful, flexible command-line tool for interrogating DNS name servers. It performs DNS lookups and displays the answers returned from the queried name servers. While similar to `nslookup`, `dig` provides much more detailed output by default and is widely preferred by Linux system administrators for DNS troubleshooting. Using the `+short` flag strips the verbose metadata and returns only the resolved IP addresses.