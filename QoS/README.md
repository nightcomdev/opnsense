# Firewall QoS shaper
All settings are the same for Upload/Download. You can also play with `Quantum` for example `1514`. This is simplest way with 1Gbps fiber to provide QoS. More complex solution on the way...

I uploaded my current settings of QoS, it's working like that over 2 weeks and no issues at all even during heavy load of network. `Buckets` are 2048, default `Flow` is 1024, so if you set `Flow` 2048 then set also `Buckets` to 4096.

## Agressive values
![alt text](https://github.com/nightcomdev/opnsense/blob/main/QoS/images/pipe.PNG "Pipe Download")

![alt text](https://github.com/nightcomdev/opnsense/blob/main/QoS/images/queue.PNG "Queue Download")

![alt text](https://github.com/nightcomdev/opnsense/blob/main/QoS/images/rules.PNG "Rules Download")

![alt text](https://github.com/nightcomdev/opnsense/blob/main/QoS/images/rules-upload.PNG "Rules Download")
## Conservative values




