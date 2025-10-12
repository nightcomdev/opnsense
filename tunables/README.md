# TUNABLES optimization for OPNsense and i226-V
Here you can find tunables that I optimazed for my network. Default values are values used by Opnsense. All informations and descriptions are from internet like blogs, forums, documentation of OPNsense and FreeBSD.

Content will be added with time, please be patient. 

Read carefully and adjust to your needs. If you have stronger hardware you can rise buffers and adjust some other values. Network card Intel i226-V was upgraded to firmware v2.32

#### Before any changes make BACKUP of your current config!

# OPNsense System Tunables Configuration
This configuration is tuned for router Hunsn RJ03 with OPNsense. Hardware list:
- CPU N5105
- 16GB RAM
- 4x i226-V
- M.2 SSD
- ZFS
- internet 1Gbps fiber Upload/Download
- QoS 920Mbps Upload/Download
- LAN 2.5Gbps

## Network Security & Hardening

| Tunable                        | Value | Default | Description                                   |
| ------------------------------ |:-----:|:-------:| --------------------------------------------- |
| net.inet.tcp.drop_synfin       | 1     | 0       | Drop SYN-FIN packets (breaks RFC1379)         |
| net.inet6.ip6.redirect         | 0     | 1       | Disable sending IPv6 redirects                |
| net.inet.tcp.syncookies        | 1     | 1       | SYN cookies for SYN-ACK packets               |
| net.inet.tcp.delayed_ack       | 0     | 1       | Disable delayed ACK piggybacking              |
| net.inet.icmp.drop_redirect    | 1     | 0       | Drop all inbound ICMP redirect packets        |
| security.bsd.see_other_gids    | 0     | 1       | Hide processes from other groups              |
| security.bsd.see_other_uids    | 0     | 1       | Hide processes from other users               |
| hw.ibrs_disable                | 1     | 0       | Disable Spectre V2 mitigation                 |
| net.inet.ip.redirect           | 0     | 1       | Disable sending ICMP redirects                |
| net.inet.tcp.icmp_may_rst      | 0     | 1       | Disable ICMP unreachable aborting connections |
| net.inet.ip.check_interface    | 1     | 0       | Enable interface checking                     |
| net.inet.ip.sourceroute        | 0     | 0       | Prevent source routing attacks                |
| net.inet.ip.accept_sourceroute | 0     | 0       | Prevent accepting source routed packets       |
| net.inet.icmp.log_redirect     | 0     | 0       | Prevent redirect packet log flooding          |


## Network Performance & Buffers

| Tunable                   | Value    | Default | Description                        |
| ------------------------- |:--------:|:-------:|:----------------------------------:|
| kern.ipc.maxsockbuf	      | 16777216 | 2097152 | Maximum socket buffer size (16MB)  |
| kern.ipc.nmbjumbop        | 65536    | 4096    | Max mbuf page size jumbo clusters  |
| kern.ipc.nmbclusters      | 2000000  | 65536   | Max mbuf clusters allowed          |
| net.inet.tcp.recvbuf_max	 | 4194304  | 2097152 | Max automatic receive buffer (4MB) |
| net.inet.tcp.sendbuf_max  | 4194304  | 2097152 | Max automatic send buffer (4MB)    |
| net.inet.tcp.sendspace    | 65536    | 32768   | Initial send socket buffer size    |
| net.inet.tcp.recvspace    | 65536    | 65536   | Initial receive socket buffer size |
| net.inet.udp.recvspace    | 131072    | 42080   | Max incoming UDP datagram space    |
| net.inet.udp.sendspace    | 131072    | 9216    | Max outgoing UDP datagram space    |
| net.inet.udp.maxdgram     | 65536 | 9216    | Max outgoing UDP datagram size       |
| net.local.dgram.maxdgram  | 2048  | 2048    | Max outgoing local UDP datagram size |
| net.inet.tcp.sendbuf_inc	 | 65536 | 8192    | Send buffer increment step size      |
| net.inet.tcp.minmss       | 536   | 216     | Minimum TCP Maximum Segment Size     |
| net.link.ifqmaxlen        | 1024  | 50      | Max send queue size                  |
| kern.ipc.maxsockets	      | 65536 | 65536 | Maximum number of sockets              |


## TCP Optimization & Algorithms

| Tunable                       | Value | Default | Description                            |
| ----------------------------- |:-----:|:-------:|:--------------------------------------:|
| net.inet.tcp.tso              | 0     | 1       | Disable TCP Segmentation Offload       |
| net.inet.tcp.soreceive_stream | 1     | 0       | Use soreceive_stream for TCP           |
| net.inet.tcp.rfc1323          | 1     | 1       | Enable high performance TCP extensions |
| net.inet.tcp.sendbuf_auto     | 1     | 1       | Automatic send buffer sizing           |
| net.inet.tcp.recvbuf_auto     | 1     | 1       | Automatic receive buffer sizing        |
| net.inet.tcp.fastopen         | 1     | 0       | Enable TCP Fast Open                   |
| net.inet.tcp.sack.enable      | 1     | 1       | Enable TCP Selective ACK               |
| net.inet.tcp.cc.algorithm      | cubic | newreno | Congestion control algorithm         |
| net.inet.tcp.cc.abe            | 1     | 0       | TCP Alternative Backoff with ECN     |
| net.inet.tcp.rfc6675_pipe      | 1     | 0       | RFC6675 pipe calculation             |
| net.inet.tcp.abc_l_var         | 44    | 2       | Max cwnd increment during slow-start |
| net.inet.tcp.initcwnd_segments | 44    | 10      | Initial congestion window segments   |
| net.inet.tcp.inflight.min      | 6144  | 6144    | Minimum in-flight data               |
| net.inet.tcp.inflight.enable   | 1     | 0       | Enable in-flight tracking            |


## Hardware & Driver Settings

| Tunable                   | Value | Default | Description                                 |
| ------------------------- |:-----:|:-------:|:-------------------------------------------:|
| dev.igc.0.fc              | 0     | 1       | Disable flow control on igc0                |
| dev.igc.1.fc              | 0     | 1       | Disable flow control on igc1                |
| dev.igc.2.fc              | 0     | 1       | Disable flow control on igc2                |
| dev.igc.3.fc              | 0     | 1       | Disable flow control on igc3                |
| dev.igc.0.eee_control     | 0     | 1       | Disable Energy Efficient Ethernet           |
| dev.igc.1.eee_control     | 0     | 1       | Disable Energy Efficient Ethernet           |
| dev.igc.2.eee_control     | 0     | 1       | Disable Energy Efficient Ethernet           |
| dev.igc.3.eee_control     | 0     | 1       | Disable Energy Efficient Ethernet           |
| hw.igc.max_interrupt_rate | 10000 | 8000    | Max interrupts per second (8k) You can test with higher values if you have better ethernet card |
| hw.igc.enable_aim         | 2     | 1       | Adaptive interrupt moderation (2=low latency, 1=normal) |


## System & Process Management

| Tunable                   | Value | Default | Description                                 |
| ------------------------- |:-----:|:-------:|:-------------------------------------------:|
| kern.sched.preempt_thresh | 240   | 0       | Max priority for preemption                 |
| kern.ipc.soacceptqueue    | 1024  | 128     | Max listen socket queue size                |
| kern.randompid	          | 1     | 0       | Randomize process IDs                       |
| hw.syscons.kbd_reboot	    | 0     | 1       | Disable CTRL+ALT+DEL reboot                 |
| kern.random.fortuna.minpoolsize	| 128 | 64 | Min entropy pool size for reseed             |
| kern.random.harvest.mask	| 33119 | 511    | Entropy harvesting mask                      |


## Network Stack & RSS

| Tunable                     | Value  | Default  | Description                    |
| --------------------------- |:------:|:--------:|:------------------------------:|
| net.isr.dispatch            | hybrid | deferred | NetISR dispatch policy         |
| net.isr.bindthreads         | 1      | 0        | Bind netisr threads to CPUs    |
| Bind netisr threads to CPUs | 4      | 1        | Max CPUs for netisr processing |
| net.inet.rss.enabled        | 1      | 0        | Enable Receive Side Scaling    |
| net.inet.rss.bits           | 2      | 0        | RSS bits configuration         |
| net.inet.ip.fastforwarding	| 1      | 0        | Enable IP fast forwarding      |
| net.isr.defaultqlimit       | 512    | 256      | Default netisr queue limit     |
| net.isr.direct_force	      | 0      | 0        | Force direct netisr dispatch. This one you can remove unless you will use direct instead hybrid |
| net.route.netisr_maxqlen	  | 512    | 1024     | Max routing socket queue length |


## IPv6 Privacy & Configuration

| Tunable                     | Value  | Default  | Description                    |
| --------------------------- |:------:|:--------:|:------------------------------:|
| net.inet6.ip6.use_tempaddr	| 1      | 0        | Enable IPv6 privacy addresses (RFC 4941) |
| net.inet6.ip6.prefer_tempaddr | 1    | 0        | Prefer privacy addresses over normal     |


## Memory & ARC (ZFS)

| Tunable                     | Value  | Default  | Description                    |
| --------------------------- |:------:|:--------:|:------------------------------:|
| vfs.zfs.arc_max	            | 2147483648 | Auto | Max ARC size in bytes (2GB)    |
| vfs.zfs.arc_min	            | 536870912  | Auto | Min ARC size in bytes (512MB)  |
| vfs.read_max	              | 128        |      | Improves read-ahead behavior for proxy / Unbound / local caching |


## ICMP & Multicast

| Tunable                     | Value  | Default  | Description                    |
| --------------------------- |:------:|:--------:|:------------------------------:|
| net.inet.icmp.icmplim	      | 1000   | 200      | ICMP packet rate limit         |
| net.inet.icmp.bmcastecho	  | 0      | 1        | Disable multicast echo replies |
| net.inet.icmp.maskrepl	    | 0      | 1        | Disable mask request replies   |


## Bridge & TAP Settings

| Tunable                     | Value  | Default  | Description                    |
| --------------------------- |:------:|:--------:|:------------------------------:|
| net.link.bridge.pfil_onlyip	| 1      | 0        | Handle only IP packets in bridge |
| net.link.bridge.pfil_local_phys	| 1  | 0        | Filter on physical interface   |
| net.link.bridge.pfil_member	 | 0     | 1        | Disable filtering on member interfaces |
| net.link.bridge.pfil_bridge	 | 1     | 0        | Enable filtering on bridge interface |
| net.link.tap.user_open	     | 1     | 0        | Allow unprivileged TAP access |
| net.link.ether.inet.max_age	 | 1200  | 1200     | ARP entry lifetime in seconds. You can lower 600-300 if using HA/CARP |


## Network Interface Settings

# Tunables for 1Gbps Network

Tunables optimized for 1Gbps network, CPU N5105, Intel i226-V and 16GB RAM

---

### <span style="color: rgb(53, 152, 219);">**Network Security &amp; Hardening**</span>

<div class="ds-scroll-area _1210dd7" id="bkmrk-tunable-gui-value-op"><table><thead><tr><th>Tunable</th><th>GUI Value</th><th>OPNsense Default</th><th>Description</th></tr></thead><tbody><tr><td>`net.inet.ip.sourceroute`</td><td>0</td><td>0</td><td>Prevent source routing attacks</td></tr><tr><td>`net.inet.ip.accept_sourceroute`</td><td>0</td><td>0</td><td>Prevent accepting source routed packets</td></tr><tr><td>`net.inet.icmp.log_redirect`</td><td>0</td><td>0</td><td>Prevent redirect packet log flooding</td></tr><tr><td>`net.inet.tcp.drop_synfin`</td><td>1</td><td>0</td><td>Drop SYN-FIN packets (breaks RFC1379)</td></tr><tr><td>`net.inet6.ip6.redirect`</td><td>0</td><td>1</td><td>Disable sending IPv6 redirects</td></tr><tr><td>`net.inet.tcp.syncookies`</td><td>1</td><td>1</td><td>SYN cookies for SYN-ACK packets</td></tr><tr><td>`net.inet.tcp.delayed_ack`</td><td>0</td><td>1</td><td>Disable delayed ACK piggybacking</td></tr><tr><td>`net.inet.icmp.drop_redirect`</td><td>1</td><td>0</td><td>Drop all inbound ICMP redirect packets</td></tr><tr><td>`security.bsd.see_other_gids`</td><td>0</td><td>1</td><td>Hide processes from other groups</td></tr><tr><td>`security.bsd.see_other_uids`</td><td>0</td><td>1</td><td>Hide processes from other users</td></tr><tr><td>`hw.ibrs_disable`</td><td>1</td><td>0</td><td>Disable Spectre V2 mitigation</td></tr><tr><td>`net.inet.ip.redirect`</td><td>0</td><td>1</td><td>Disable sending ICMP redirects</td></tr><tr><td>`net.inet.tcp.icmp_may_rst`</td><td>0</td><td>1</td><td>Disable ICMP unreachable aborting connections</td></tr><tr><td>`net.inet.ip.check_interface`</td><td>1</td><td>0</td><td>Enable interface checking</td></tr></tbody></table>

</div>### <span style="color: rgb(53, 152, 219);">**IPv6 Privacy &amp; Configuration**</span>

<div class="ds-scroll-area _1210dd7" id="bkmrk-tunable-gui-value-op-1"><table><thead><tr><th>Tunable</th><th>GUI Value</th><th>OPNsense Default</th><th>Description</th></tr></thead><tbody><tr><td>`net.inet6.ip6.use_tempaddr`</td><td>1</td><td>0</td><td>Enable IPv6 privacy addresses (RFC 4941)</td></tr><tr><td>`net.inet6.ip6.prefer_tempaddr`</td><td>1</td><td>0</td><td>Prefer privacy addresses over normal</td></tr></tbody></table>

</div>### <span style="color: rgb(53, 152, 219);">**Network Performance &amp; Buffers**</span>

<div class="ds-scroll-area _1210dd7" id="bkmrk-tunable-gui-value-op-2"><table><thead><tr><th>Tunable</th><th>GUI Value</th><th>OPNsense Default</th><th>Description</th></tr></thead><tbody><tr><td>`net.inet.udp.maxdgram`</td><td>65536</td><td>9216</td><td>Max outgoing UDP datagram size</td></tr><tr><td>`kern.ipc.maxsockbuf`</td><td>16777216</td><td>2097152</td><td>Maximum socket buffer size (16MB)</td></tr><tr><td>`kern.ipc.nmbjumbop`</td><td>65536</td><td>4096</td><td>Max mbuf page size jumbo clusters</td></tr><tr><td>`kern.ipc.nmbclusters`</td><td>2000000</td><td>65536</td><td>Max mbuf clusters allowed</td></tr><tr><td>`net.inet.tcp.recvbuf_max`</td><td>4194304</td><td>2097152</td><td>Max automatic receive buffer (4MB)</td></tr><tr><td>`net.inet.tcp.sendbuf_max`</td><td>4194304</td><td>2097152</td><td>Max automatic send buffer (4MB)</td></tr><tr><td>`net.inet.tcp.sendspace`</td><td>65536</td><td>32768</td><td>Initial send socket buffer size</td></tr><tr><td>`net.inet.tcp.recvspace`</td><td>65536</td><td>65536</td><td>Initial receive socket buffer size</td></tr><tr><td>`net.inet.udp.recvspace`</td><td>131072</td><td>42080</td><td>Max incoming UDP datagram space</td></tr><tr><td>`net.inet.udp.sendspace`</td><td>131072</td><td>9216</td><td>Max outgoing UDP datagram space</td></tr><tr><td>`net.local.dgram.maxdgram`</td><td>2048</td><td>2048</td><td>Max outgoing local UDP datagram size</td></tr><tr><td>`net.inet.tcp.sendbuf_inc`</td><td>65536</td><td>8192</td><td>Send buffer increment step size</td></tr><tr><td>`net.inet.tcp.minmss`</td><td>536</td><td>216</td><td>Minimum TCP Maximum Segment Size</td></tr><tr><td>`net.link.ifqmaxlen`</td><td>1024</td><td>50</td><td>Max send queue size</td></tr><tr><td>`kern.ipc.maxsockets`</td><td>65536</td><td>65536</td><td>Maximum number of sockets</td></tr></tbody></table>

</div>### <span style="color: rgb(53, 152, 219);">**TCP Optimization &amp; Algorithms**</span>

<div class="ds-scroll-area _1210dd7" id="bkmrk-tunable-gui-value-op-3"><table><thead><tr><th>Tunable</th><th>GUI Value</th><th>OPNsense Default</th><th>Description</th></tr></thead><tbody><tr><td>`net.inet.tcp.tso`</td><td>0</td><td>1</td><td>Disable TCP Segmentation Offload</td></tr><tr><td>`net.inet.tcp.soreceive_stream`</td><td>1</td><td>0</td><td>Use soreceive\_stream for TCP</td></tr><tr><td>`net.inet.tcp.rfc1323`</td><td>1</td><td>1</td><td>Enable high performance TCP extensions</td></tr><tr><td>`net.inet.tcp.sendbuf_auto`</td><td>1</td><td>1</td><td>Automatic send buffer sizing</td></tr><tr><td>`net.inet.tcp.recvbuf_auto`</td><td>1</td><td>1</td><td>Automatic receive buffer sizing</td></tr><tr><td>`net.inet.tcp.cc.algorithm`</td><td>cubic</td><td>newreno</td><td>Congestion control algorithm</td></tr><tr><td>`net.inet.tcp.fastopen`</td><td>1</td><td>0</td><td>Enable TCP Fast Open</td></tr><tr><td>`net.inet.tcp.sack.enable`</td><td>1</td><td>1</td><td>Enable TCP Selective ACK</td></tr><tr><td>`net.inet.tcp.cc.abe`</td><td>1</td><td>0</td><td>TCP Alternative Backoff with ECN</td></tr><tr><td>`net.inet.tcp.rfc6675_pipe`</td><td>1</td><td>0</td><td>RFC6675 pipe calculation</td></tr><tr><td>`net.inet.tcp.abc_l_var`</td><td>44</td><td>2</td><td>Max cwnd increment during slow-start</td></tr><tr><td>`net.inet.tcp.initcwnd_segments`</td><td>44</td><td>10</td><td>Initial congestion window segments</td></tr><tr><td>`net.inet.tcp.inflight.min`</td><td>6144</td><td>6144</td><td>Minimum in-flight data</td></tr><tr><td>`net.inet.tcp.inflight.enable`</td><td>1</td><td>0</td><td>Enable in-flight tracking</td></tr></tbody></table>

</div>### <span style="color: rgb(53, 152, 219);">**Hardware &amp; Driver Settings**</span>

<div class="ds-scroll-area _1210dd7" id="bkmrk-tunable-gui-value-op-4"><table><thead><tr><th>Tunable</th><th>GUI Value</th><th>OPNsense Default</th><th>Description</th></tr></thead><tbody><tr><td>`dev.igc.0.fc`</td><td>0</td><td>3</td><td>Disable flow control on igc0</td></tr><tr><td>`dev.igc.1.fc`</td><td>0</td><td>3</td><td>Disable flow control on igc1</td></tr><tr><td>`dev.igc.2.fc`</td><td>0</td><td>3</td><td>Disable flow control on igc2</td></tr><tr><td>`dev.igc.3.fc`</td><td>0</td><td>3</td><td>Disable flow control on igc3</td></tr><tr><td>`dev.igc.0.eee_control`</td><td>0</td><td>1</td><td>Disable Energy Efficient Ethernet</td></tr><tr><td>`dev.igc.1.eee_control`</td><td>0</td><td>1</td><td>Disable Energy Efficient Ethernet</td></tr><tr><td>`dev.igc.2.eee_control`</td><td>0</td><td>1</td><td>Disable Energy Efficient Ethernet</td></tr><tr><td>`dev.igc.3.eee_control`</td><td>0</td><td>1</td><td>Disable Energy Efficient Ethernet</td></tr><tr><td>`hw.igc.max_interrupt_rate`</td><td>8000</td><td>100000</td><td>Max interrupts per second (8k)</td></tr><tr><td>`hw.igc.enable_aim`</td><td>2</td><td>1</td><td>Adaptive interrupt moderation (low latency)</td></tr><tr><td>`hw.igc.rx_process_limit`</td><td>-1</td><td>100</td><td>RX process limit (unlimited)</td></tr><tr><td>`hw.igc.tx_process_limit`</td><td>-1</td><td>100</td><td>TX process limit (unlimited)</td></tr><tr><td>`dev.igc.0.tx_abs_int_delay`</td><td>0</td><td>0</td><td>TX absolute interrupt delay</td></tr><tr><td>`dev.igc.1.tx_abs_int_delay`</td><td>0</td><td>0</td><td>TX absolute interrupt delay</td></tr><tr><td>`dev.igc.0.rx_abs_int_delay`</td><td>0</td><td>0</td><td>RX absolute interrupt delay</td></tr><tr><td>`dev.igc.1.rx_abs_int_delay`</td><td>0</td><td>0</td><td>RX absolute interrupt delay</td></tr><tr><td>`dev.igc.0.rxd_head_wb_enable`</td><td>1</td><td>0</td><td>Enable RX descriptor head write-back</td></tr><tr><td>`dev.igc.1.rxd_head_wb_enable`</td><td>1</td><td>0</td><td>Enable RX descriptor head write-back</td></tr></tbody></table>

</div>### <span style="color: rgb(53, 152, 219);">**System &amp; Process Management**</span>

<div class="ds-scroll-area _1210dd7" id="bkmrk-tunable-gui-value-op-5"><table><thead><tr><th>Tunable</th><th>GUI Value</th><th>OPNsense Default</th><th>Description</th></tr></thead><tbody><tr><td>`kern.randompid`</td><td>1</td><td>0</td><td>Randomize process IDs</td></tr><tr><td>`hw.syscons.kbd_reboot`</td><td>0</td><td>1</td><td>Disable CTRL+ALT+DEL reboot</td></tr><tr><td>`kern.sched.preempt_thresh`</td><td>240</td><td>0</td><td>Max priority for preemption</td></tr><tr><td>`kern.ipc.soacceptqueue`</td><td>1024</td><td>128</td><td>Max listen socket queue size</td></tr><tr><td>`kern.random.fortuna.minpoolsize`</td><td>128</td><td>64</td><td>Min entropy pool size for reseed</td></tr><tr><td>`kern.random.harvest.mask`</td><td>33119</td><td>511</td><td>Entropy harvesting mask</td></tr></tbody></table>

</div>### <span style="color: rgb(53, 152, 219);">**Network Stack &amp; RSS**</span>

<div class="ds-scroll-area _1210dd7" id="bkmrk-tunable-gui-value-op-6"><table><thead><tr><th>Tunable</th><th>GUI Value</th><th>OPNsense Default</th><th>Description</th></tr></thead><tbody><tr><td>`net.isr.dispatch`</td><td>hybrid</td><td>deferred</td><td>NetISR dispatch policy</td></tr><tr><td>`net.isr.bindthreads`</td><td>1</td><td>0</td><td>Bind netisr threads to CPUs</td></tr><tr><td>`net.isr.maxthreads`</td><td>4</td><td>1</td><td>Max CPUs for netisr processing</td></tr><tr><td>`net.inet.rss.enabled`</td><td>1</td><td>0</td><td>Enable Receive Side Scaling</td></tr><tr><td>`net.inet.rss.bits`</td><td>2</td><td>0</td><td>RSS bits configuration</td></tr><tr><td>`net.inet.ip.fastforwarding`</td><td>1</td><td>0</td><td>Enable IP fast forwarding</td></tr><tr><td>`net.isr.defaultqlimit`</td><td>512</td><td>256</td><td>Default netisr queue limit</td></tr><tr><td>`net.isr.direct_force`</td><td>0</td><td>0</td><td>Force direct netisr dispatch</td></tr><tr><td>`net.route.netisr_maxqlen`</td><td>512</td><td>1024</td><td>Max routing socket queue length</td></tr></tbody></table>

</div>### <span style="color: rgb(53, 152, 219);">**Memory &amp; ARC (ZFS)**</span>

<div class="ds-scroll-area _1210dd7" id="bkmrk-tunable-gui-value-op-7"><table><thead><tr><th>Tunable</th><th>GUI Value</th><th>OPNsense Default</th><th>Description</th></tr></thead><tbody><tr><td>`vfs.zfs.arc_max`</td><td>2147483648</td><td>Auto</td><td>Max ARC size in bytes (2GB)</td></tr><tr><td>`vfs.zfs.arc_min`</td><td>536870912</td><td>Auto</td><td>Min ARC size in bytes (512MB)</td></tr><tr><td class="align-center">`vfs.read_max`</td><td class="align-center">128</td><td>  
</td><td>Improves read-ahead behavior for proxy / Unbound / local caching.</td></tr></tbody></table>
