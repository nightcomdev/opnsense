# TUNABLES optimization for Opnsense
Here you can find tunables that I optimazed for my network. Default values are values used by Opnsense. All informations and descritopns are from internet like blogs, forums, documentation of Opnsense and FreeBSD.

Read carefully and adjust to your needs. If you have stronger hardware you can rise buffers and adjust some other values. Network card Intel i226-V was upgraded to firmware v2.32

# OPNsense System Tunables Configuration

This configuration is tuned for Opnsense router with 
- CPU N5105
- 16GB RAM
- i226-V
- ZFS file system
- internet 1Gbps fiber Upload/Download

## Network Security & Hardening

| Tunable                      | Value | Default | Description                            |
| ---------------------------- |:-----:|:-------:|:--------------------------------------:|
| net.inet.tcp.delayed_ack	    | 0     | 1       | Prevent source routing attacks         |
| net.inet.icmp.drop_redirect	 | 1     | 0       | Drop all inbound ICMP redirect packets |
| hw.ibrs_disable	             | 1     | 1       | Disable Spectre V2 mitigation          |

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
| net.inet.udp.recvspace    | 65536    | 42080   | Max incoming UDP datagram space    |
| net.inet.udp.sendspace    | 65536    | 9216    | Max outgoing UDP datagram space    |

## TCP Optimization

| Tunable                       | Value | Default | Description                            |
| ----------------------------- |:-----:|:-------:|:--------------------------------------:|
| net.inet.tcp.tso              | 0     | 1       | Disable TCP Segmentation Offload       |
| net.inet.tcp.soreceive_stream | 1     | 0       | Use soreceive_stream for TCP           |
| net.inet.tcp.rfc1323          | 1     | 1       | Enable high performance TCP extensions |
| net.inet.tcp.sendbuf_auto     | 1     | 1       | Automatic send buffer sizing           |
| net.inet.tcp.recvbuf_auto     | 1     | 1       | Automatic receive buffer sizing        |
| net.inet.tcp.fastopen         | 1     | 0       | Enable TCP Fast Open                   |
| net.inet.tcp.sack.enable      | 1     | 1       | Enable TCP Selective ACK               |
