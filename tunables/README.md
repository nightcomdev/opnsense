## TUNABLES
Info related to tunables

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
