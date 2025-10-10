# TUNABLES optimization for Opnsense
Here you can find tunables that I optimazed for my network. Default values are values used by Opnsense. All informations and descritopns are from internet like blogs, forums, documentation of Opnsense and FreeBSD.

Read carefully and adjust to your needs. If you have stronger hardware you can rise buffers and adjust some other tweaks

# OPNsense System Tunables Configuration

This configuration is tuned for Opnsense router with 
- CPU N5105
- 16GB RAM
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
