# TUNABLES optimization for OPNsense and i226-V
Here you can find tunables that I optimazed for my network. `Default` values are values used by OPNsense. All informations and descriptions are from internet like blogs, forums, documentation of OPNsense and FreeBSD - detalis you can find in bottom section **Credits & Sources** on main page of this repository.

Read carefully and adjust to your needs. If you have stronger hardware you can rise buffers and adjust some other values. Network card Intel i226-V (integrated) was upgraded to firmware v2.32 from v2.17. Network with this adjustments have still plenty of space to breathe I guess it can also operate on 2.5Gbps internet speed. Only I would adjust buffers like `kern.ipc.maxsockbuf`, `net.inet.tcp.recvbuf_max`....etc.

> [!CAUTION]
> **Before any changes make BACKUP of your current config!**

# OPNsense System Tunables Configuration
This configuration is tuned for router Hunsn RJ03 with OPNsense. Hardware list:
- CPU N5105
- 16GB RAM
- 4x i226-V
- M.2 SSD
- ZFS

Other Network data:
- internet 1Gbps fiber Upload/Download
- QoS 920Mbps Upload/Download
- LAN 2.5Gbps
- 40 devices Zigbee
- 5 Virtual Machines
- NAS
- 7 smart devices

# Tunables settings
## Network Security & Hardening

| Tunable                        | Value | Default | Description                                   |
| ------------------------------ |:-----:|:-------:| --------------------------------------------- |
| `net.inet.tcp.drop_synfin`       | 1     | 0       | Drop SYN-FIN packets (breaks RFC1379). When enabled (set to 1), the TCP stack will drop incoming TCP packets that have both the SYN and FIN flags set. In effect, this is a hardening/anti-anomaly measure: a SYN+FIN combination is nonsensical in normal TCP behaviour (attempting to open and immediately close a connection) and may be used for scanning, malicious probing or anomalous traffic |
| `net.inet6.ip6.redirect`         | 0     | 1       | Disable sending IPv6 redirects. You can enable it if you have more then one router or two internal routers on the same VLAN |
| `net.inet.tcp.syncookies`        | 1     | 1       | SYN cookies for SYN-ACK packets. This enables TCP SYN cookies. When a SYN flood (many connection requests) occurs and the TCP SYN cache (partial connection table) is full or overloaded, instead of allocating full state for each incoming SYN, the system will respond with a SYN-ACK embedding a “cookie” (encoded info). When the ACK comes back, no large state was held for the SYN-RECEIVED state. This helps protect against SYN-flood denial‐of‐service attacks |
| `net.inet.tcp.delayed_ack`       | 0     | 1       | Disable delayed ACK piggybacking. `0` send an ACK immediately for every segment. Helps responsiveness and small-packet workloads (like gaming, DNS, control traffic) `1` enable delayed ACKs (wait a short time before replying). Helps large file transfers or high-connection servers by sending fewer ACKs. |
| `net.inet.icmp.drop_redirect`    | 1     | 0       | Drop all inbound ICMP redirect packets. This tunable tells the kernel whether to accept or drop ICMP Redirect messages from other devices. `0`=accept (Default) `1`=drop it means OPNsense will simply ignore all ICMP Redirects, routing tables stay fixed, no chance of redirect-based spoofing or route confusion and no more “ICMP redirect” messages in logs. This does not break normal ICMP functions (ping, traceroute, etc.). It only ignores “redirect” control messages |
| `security.bsd.see_other_gids`    | 0     | 1       | Hide processes from other groups              |
| `security.bsd.see_other_uids`    | 0     | 1       | Hide processes from other users               |
| `hw.ibrs_disable`                | 1     | 0       | Disable Spectre V2 mitigation `0`=enabled  `1`=disabled |
| `net.inet.ip.redirect`           | 0     | 1       | Disable sending ICMP redirects `0`=disabled  `1`=enabled |
| `net.inet.tcp.icmp_may_rst`      | 0     | 1       | Disable ICMP unreachable aborting connections `0`=disabled  `1`=enabled |
| `net.inet.ip.check_interface`    | 1     | 0       | Enable interface checking `0`=disabled  `1`=enabled. When enabled, this setting causes the IP-stack to verify that an incoming IPv4 packet arrives on the interface that actually has the destination address (i.e., the packet’s destination IP must match an address configured on the receiving interface) before processing it |
| `net.inet.ip.sourceroute`        | 0     | 0       | Prevent source routing attacks                |
| `net.inet.ip.accept_sourceroute` | 0     | 0       | Prevent accepting source routed packets       |
| `net.inet.icmp.log_redirect`     | 0     | 0       | Prevent redirect packet log flooding          |


## Network Performance & Buffers

| Tunable                   | Value    | Default | Description                        |
| ------------------------- |:--------:|:-------:|:----------------------------------:|
| `kern.ipc.maxsockbuf`	      | 33554432 | 2097152 | Maximum socket buffer size (32MB), default 2MB. Sets the maximum total socket buffer size (send + receive) allowed for any TCP/UDP socket in the kernel. It defines the upper ceiling for: `net.inet.tcp.sendbuf_max`, `net.inet.tcp.recvbuf_max`, `net.inet.udp.recvspace`, `net.inet.udp.sendspace`  |
| `kern.ipc.nmbjumbop`        | 65536    | 4096    | Max mbuf page size jumbo clusters. Sets the number of jumbo network mbuf clusters (16 KB buffers) that the kernel pre-allocates for large network packets and high-throughput workloads. Related with: `kern.ipc.nmbclusters`, `kern.ipc.nmbjumbo9`, `kern.ipc.nmbjumbop`, `kern.ipc.nmbufs` |
| `kern.ipc.nmbclusters`      | 200000  | 65536   | Max mbuf clusters allowed          |
| `net.inet.tcp.recvbuf_max`	| 6291456  | 2097152 | Max automatic receive buffer (6MB), default 2MB. This sets the maximum size of the TCP socket **receive buffer** that the kernel will allow for a connection. It limits how much TCP data can be buffered on the receiving side before being processed by the application or kernel. |
| `net.inet.tcp.sendbuf_max`  | 6291456  | 2097152 | Max automatic send buffer (6MB), default 2MB. This sets the maximum size of the TCP socket send buffer that the kernel will allow for a connection. It limits how much data the kernel can buffer for sending (before the application or network “catches up”). |
| `net.inet.tcp.sendspace`    | 131072    | 32768   | Initial send socket buffer size    |
| `net.inet.tcp.recvspace`    | 131072    | 65536   | Initial receive socket buffer size |
| `net.inet.udp.recvspace`    | 131072    | 42080   | Max incoming UDP datagram space    |
| `net.inet.udp.sendspace`    | 65536    | 9216    | Max outgoing UDP datagram space    |
| `net.inet.udp.maxdgram`     | 65536 | 9216    | Max outgoing UDP datagram size       |
| `net.local.dgram.maxdgram`  | 2048  | 2048    | Max outgoing local UDP datagram size |
| `net.inet.tcp.recvbuf_inc`  | 65536 | 16384   | Receive buffer increment step size. This parameter sets the increment size by which the kernel will grow the TCP socket receive buffer (when “auto-tuning” is enabled) for a given connection. In other words: if receive buffer auto-growth is turned on (via `net.inet.tcp.recvbuf_auto = 1`), the system starts with a smaller buffer size and, if needed (based on congestion, RTT, throughput, etc), the buffer will step up in increments of `recvbuf_inc` until the cap of `net.inet.tcp.recvbuf_max` is reached |
| `net.inet.tcp.sendbuf_inc`	| 32768 | 8192    | Send buffer increment step size. Similar in concept to the receive side: this parameter sets the increment size by which the kernel will grow the TCP socket send buffer (when auto‐tuning is enabled via `net.inet.tcp.sendbuf_auto = 1`). It determines how aggressively the send buffer can expand for a given connection, up to the cap `net.inet.tcp.sendbuf_max` |
| `net.inet.tcp.minmss`       | 536   | 216     | Minimum TCP Maximum Segment Size     |
| `net.link.ifqmaxlen`        | 1024  | 512      | Max send queue size. Default maximum length (in packets) of each interface transmit queue. When a process, kernel subsystem, or firewall rule wants to send a packet, it first places it into the interface’s output queue. Set `512` if you look for very low latency. |
| `kern.ipc.maxsockets`	      | 65536 | 65536 | Maximum number of sockets              |
| `kern.ipc.soacceptqueue`    | 1024  | 128   | Defines the maximum number of pending TCP connections that can wait in a listen backlog before being accepted by a user process (like Unbound, Web UI, or a local daemon) |
| `net.inet.tcp.acc_ecn`      | 1     |       |                                        |

## TCP Optimization & Algorithms

| Tunable                       | Value | Default | Description                            |
| ----------------------------- |:-----:|:-------:|:--------------------------------------:|
| `net.inet.tcp.tso`              | 0     | 1       | Disable TCP Segmentation Offload. `1` (enabled) Lower CPU, – slightly higher latency, – can cause bursty traffic / `0` (disabled) Lower jitter and latency, + better fairness under FQ-CoDel, – slightly more CPU use.        |
| `net.inet.tcp.soreceive_stream` | 1     | 0       | Use soreceive_stream for TCP. `0` Use the traditional BSD socket receive path (called “soreceive_dgram”) / `1` Use the streamlined TCP receive path (“soreceive_stream”), optimized for continuous byte streams          |
| `net.inet.tcp.rfc1323`          | 1     | 1       | Enable high performance TCP extensions |
| `net.inet.tcp.sendbuf_auto`     | 1     | 1       | Automatic send buffer sizing           |
| `net.inet.tcp.recvbuf_auto`     | 1     | 1       | Automatic receive buffer sizing        |
| `net.inet.tcp.fastopen`         | 1     | 0       | Enable TCP Fast Open                   |
| `net.inet.tcp.sack.enable`      | 1     | 1       | Enable TCP Selective ACK               |
| `net.inet.tcp.cc.algorithm`      | cubic | newreno | TCP Congestion control algorithm. `newreno` Classic, very conservative growth. Low bandwidth, stable links, legacy devices. `htcp` Faster ramp-up, fairness improvements over newreno. Medium-to-high latency links. `cubic` Default in Linux and FreeBSD; fast, stable, scalable. Modern broadband and datacenter links. `bbr` Model-based (Google’s Bottleneck Bandwidth & RTT) — aims to minimize bufferbloat. Long-RTT or lossy links; can be “too aggressive” on low-latency fiber |
| `net.inet.tcp.cc.abe`            | 1     | 0       | TCP Alternative Backoff with ECN. Accurate ECN (Explicit Congestion Notification) Behavior Enhancement. Normally, TCP uses packet loss as the only signal of congestion. With ECN enabled, routers can mark packets instead of dropping them when the queue begins to fill — telling the sender to slow down slightly before any loss occurs. Good combinantion with FQ-CoDel shaping + ECN enabled in queues + CUBIC congestion control that supports ABE extensions |
| `net.inet.tcp.rfc6675_pipe`      | 1     | 0       | RFC6675 pipe calculation             |
| `net.inet.tcp.abc_l_var`         | 44    | 2       | Max cwnd increment during slow-start |
| `net.inet.tcp.initcwnd_segments` | 44    | 10      | Initial congestion window segments   |
| `net.inet.tcp.inflight.min`      | 6144  | 6144    | Minimum in-flight data               |
| `net.inet.tcp.inflight.enable`   | 1     | 0       | Enable in-flight tracking            |


## Hardware & Driver Settings

| Tunable                   | Value | Default | Description                                 |
| ------------------------- |:-----:|:-------:|:-------------------------------------------:|
| `dev.igc.0.fc`              | 0     | 1       | Disable flow control on igc0 `0`=disabled `1`=enabled |
| `dev.igc.1.fc`              | 0     | 1       | Disable flow control on igc1 `0`=disabled `1`=enabled |
| `dev.igc.2.fc`              | 0     | 1       | Disable flow control on igc2 `0`=disabled `1`=enabled |
| `dev.igc.3.fc`              | 0     | 1       | Disable flow control on igc3 `0`=disabled `1`=enabled |
| `dev.igc.0.eee_control`     | 0     | 1       | Disable Energy Efficient Ethernet `0`=disable `1`=enabled |
| `dev.igc.1.eee_control`     | 0     | 1       | Disable Energy Efficient Ethernet `0`=disable `1`=enabled |
| `dev.igc.2.eee_control`     | 0     | 1       | Disable Energy Efficient Ethernet `0`=disable `1`=enabled |
| `dev.igc.3.eee_control`     | 0     | 1       | Disable Energy Efficient Ethernet `0`=disable `1`=enabled |
| `hw.igc.max_interrupt_rate` | 20000 | 8000    | Max interrupts per second (10k). 8000 is also good and keep latency low. You can test with higher values if you have better ethernet card |
| `hw.igc.enable_aim`         | 2     | 1       | Adaptive interrupt moderation. `2`=low latency Adaptive Interrupt Moderation (AIM) — dynamically adjusts interrupt rate based on load., `1`=normal Static interrupt moderation — not adaptive. (Fixed delay intervals), `0`=disabled Every packet (or small group) triggers immediate interrupt. Lowest latency, highest CPU interrupt rate |
| `hw.igc.rx_process_limit`   | 1024    | 100     | The maximum number of packets the driver will process per interrupt RX. If you see latency spikes change to 1024 |
| `hw.igc.tx_process_limit`   | 1024    | 100     | The maximum number of packets the driver will process per interrupt TX. If you see latency spikes change to 1024 |
| `hw.pci.enable_aspm`        | 0     | 0       | ASPM (Active State Power Management) lets PCIe links enter low-power states (L0s or L1) when idle. This saves a few hundred milliwatts but adds small wake-up delays (microseconds) when traffic resumes. `0`=disabled `1`=enabled |
| `hw.igc.rx_abs_int_delay`   |  0    |         |                                            |
| `hw.igc.tx_abs_int_delay`   | 0     |         |                                            |


## System & Process Management

| Tunable                   | Value | Default | Description                                 |
| ------------------------- |:-----:|:-------:|:-------------------------------------------:|
| `kern.sched.preempt_thresh` | 240   | 0       | Max priority for preemption                 |
| `kern.randompid`	          | 1     | 0       | Randomize process IDs                       |
| `hw.syscons.kbd_reboot`	    | 0     | 1       | Disable CTRL+ALT+DEL reboot                 |
| `kern.random.fortuna.minpoolsize`	| 128 | 64 | Min entropy pool size for reseed             |
| `kern.random.harvest.mask`	| 33119 | 511    | Entropy harvesting mask                      |


## Network Stack & RSS

| Tunable                     | Value  | Default  | Description                    |
| --------------------------- |:------:|:--------:|:------------------------------:|
| `net.isr.dispatch`            | hybrid | deferred | NetISR dispatch policy         |
| `net.isr.bindthreads`         | 1      | 0        | Bind netisr threads to CPUs    |
| `net.isr.maxthreads`          | 4      | 1        | Max CPUs for netisr processing |
| `net.inet.rss.enabled`        | 1      | 0        | Enable Receive Side Scaling    |
| `net.inet.rss.bits`           | 2      | 0        | RSS bits configuration         |
| `net.inet.ip.fastforwarding`	| 1      | 0        | Enable IP fast forwarding      |
| `net.isr.defaultqlimit`       | 1024   | 256      | Default netisr queue limit. Defines the maximum number of packets that each software interrupt (netisr) queue can hold before new packets are dropped. Set `512` if you look for very low latency |
| `net.isr.direct_force`	      | 0      | 0        | Force direct netisr dispatch. This one you can remove unless you will use `direct` instead `hybrid`. `0`=disabled and `1`=enabled |
| `net.route.netisr_maxqlen`	  | 512    | 256     | Max routing socket queue length. Defines the maximum number of packets or routing events that can be queued in the routing netisr queue. Think of it as the work queue for the routing thread inside the network ISR (interrupt service routine) framework. Set it to `256` is you search for low latency. |


## IPv6 Privacy & Configuration

| Tunable                     | Value  | Default  | Description                    |
| --------------------------- |:------:|:--------:|:------------------------------:|
| `net.inet6.ip6.use_tempaddr`	| 1      | 0        | Enable IPv6 privacy addresses (RFC 4941) |
| `net.inet6.ip6.prefer_tempaddr` | 1    | 0        | Prefer privacy addresses over normal     |


## Memory & ARC (ZFS)

| Tunable                     | Value  | Default  | Description                    |
| --------------------------- |:------:|:--------:|:------------------------------:|
| `vfs.zfs.arc_max`	            | 2147483648 | Auto | Max ARC size in bytes (2GB)    |
| `vfs.zfs.arc_min`	            | 536870912  | Auto | Min ARC size in bytes (512MB)  |
| `vfs.read_max`	              | 128        |      | Improves read-ahead behavior for proxy / Unbound / local caching |


## ICMP & Multicast

| Tunable                     | Value  | Default  | Description                    |
| --------------------------- |:------:|:--------:|:------------------------------:|
| `net.inet.icmp.icmplim`	      | 1000   | 200      | ICMP packet rate limit         |
| `net.inet.icmp.bmcastecho`	  | 0      | 1        | Disable multicast echo replies |
| `net.inet.icmp.maskrepl`	    | 0      | 1        | Disable mask request replies   |


## Bridge & TAP Settings

| Tunable                     | Value  | Default  | Description                    |
| --------------------------- |:------:|:--------:|:------------------------------:|
| `net.link.bridge.pfil_onlyip`	| 1      | 0        | Handle only IP packets in bridge |
| `net.link.bridge.pfil_local_phys`	| 1  | 0        | Filter on physical interface   |
| `net.link.bridge.pfil_member`	 | 0     | 1        | Disable filtering on member interfaces |
| `net.link.bridge.pfil_bridge`	 | 1     | 0        | Enable filtering on bridge interface |
| `net.link.tap.user_open`	     | 1     | 0        | Allow unprivileged TAP access |
| `net.link.ether.inet.max_age`	 | 1200  | 1200     | ARP entry lifetime in seconds. You can lower 600-300 if using HA/CARP |


## Network Interface Settings

| Tunable                     | Value  | Default  | Description                    |
| --------------------------- |:------:|:--------:|:------------------------------:|
| `net.inet.ip.intr_queue_maxlen`	| 1024 | 1000       | Max IP input queue size. When a packet is handed off from the NIC driver (e.g., `igc`) to the IP input queue, it’s placed into a software queue called the IP interrupt queue (`ipintrq`) `net.inet.ip.intr_queue_maxlen` sets the maximum number of packets that this queue can hold before the kernel starts dropping new ones. |
| `net.inet6.ip6.intr_queue_maxlen`	| 1024 | 1000     | Max IPv6 input queue size      |
| `net.igc.num_queues`	         | 4      | 1       | Number of IGC queues. Value depends how many physical cores are in your CPU - number of cores used for RX/TX processing |
| `net.igc.tx_ring_size`	       | 1024   | 512     | TX ring size                   |
| `net.igc.rx_ring_size`	       | 1024   | 512     | RX ring size                   |


## Power Management & CPU

| Tunable                     | Value  | Default  | Description                    |
| --------------------------- |:------:|:--------:|:------------------------------:|
| `dev.hwpstate_intel.0.epp`    | 50     | 80       | Efficiency/Performance Preference 0=max performance / 100=most power saving |
| `dev.hwpstate_intel.1.epp`    | 50     | 80       | Efficiency/Performance Preference 0=max performance / 100=most power saving |
| `dev.hwpstate_intel.2.epp`    | 50     | 80       | Efficiency/Performance Preference 0=max performance / 100=most power saving |
| `dev.hwpstate_intel.3.epp`    | 50     | 80       | Efficiency/Performance Preference 0=max performance / 100=most power saving |
| `hw.intr_storm_threshold`	    | 5000   | 1000     | Interrupt storm protection threshold |


## PF Firewall

| Tunable                     | Value  | Default  | Description                    |
| --------------------------- |:------:|:--------:|:------------------------------:|
| `net.pf.source_nodes_hashsize`	| 65536 | 8192  | PF source nodes hashtable size |


## Legal & License

| Tunable                     | Value  | Default  | Description                    |
| --------------------------- |:------:|:--------:|:------------------------------:|
| `legal.intel_ipw.license_ack`	| 1      | 0        | Intel IPW license acknowledgment |
| `legal.intel_wpi.license_ack`	| 1      | 0        | Intel WPI license acknowledgment |
