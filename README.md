# Tuning OPNsense tunables, Unbound, QoS, Intel i226-V 
This repository contains tips & tricks, optimization and tuning of Unbound, QoS, Intel i226-V and OPNsense it self. Be aware that it's optimized for hardware listed in section **Hardware**, adjust config to your needs.
> [!CAUTION]
> Before you make any changes in your config, make BACKUP of you current config! **All changes in your config are done on your own risk!**

# Main menu 
- [Tunables](https://github.com/nightcomdev/opnsense/tree/main/tunables)
- [Unbound](https://github.com/nightcomdev/opnsense/tree/main/Unbound)
- [QoS](https://github.com/nightcomdev/opnsense/tree/main/QoS)
- [Intel i226-V Firmware upgrade](https://github.com/nightcomdev/opnsense/tree/main/i226-firmware-upgrade)



# Hardware
### Hardware used during tuning, main router Hunsn RJ03
- CPU N5105
- 16GB RAM
- 4x i226-V
- M.2 SSD
- ZFS

### Other Network data:
- internet 1Gbps fiber Upload/Download
- QoS 920Mbps Upload/Download
- LAN 2.5Gbps
- 40 devices Zigbee
- 5 Virtual Machines
- NAS
- 7 smart devices


# Credits & Sources
Here you can find links to sources from where I took all informations. Beside links I also used AI (ChatGPT & DeepSeek) to gather some informations or look for default data. Both AI's also analyzed those config files with very good feedback. Changes adviced by AI in my case didn't make any sens to data provided by OPNsense and FreeBSD documentations and could cause issues during boot or with resources. I advice NOT to follow blindly any suggestions provided by AI but instead I advice to deep dive in to documentation of OPNsense and FreeBSD and use AI as advanced search engine only!

### Sources:

- https://docs.netgate.com/pfsense/en/latest/config/advanced-tunables.html
- https://docs.opnsense.org/troubleshooting/performance.html
- https://docs.opnsense.org/troubleshooting/hardening.html
- https://www.bentasker.co.uk/posts/blog/general/opnsense-pfsense-fttp-and-1gbps-pppoe.html
- https://kings-guard.com/how-to-optimize-pfsense-plus-for-performance/
- https://docs.netgate.com/pfsense/en/latest/hardware/tune.html
- https://gist.github.com/jorisvervuurt/8ce01bb19de242484e2ec7f5c785e46b
- https://binaryimpulse.com/2022/11/opnsense-performance-tuning-for-multi-gigabit-internet/
- https://forum.opnsense.org
- https://notes.xeome.dev/notes/OPNSense-Tuning
- https://jeffmbelt.com/opnsense-1g-throughput.html
- https://windgate.net/topton-n5095-n5105-n100-opnsense-proxmox-powersave-tuning/
- https://medium.com/@truvis.thornton/opnsense-firewall-configuration-performance-tuning-for-multi-gigabit-internet-and-better-speeds-in-cfc80c49c544
- https://blog.miniserver.it/en/pfsense/tuning-and-troubleshooting-network-cards/
