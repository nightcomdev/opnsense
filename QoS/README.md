# Firewall QoS shaper
All settings are the same for Upload/Download. You can also play with `Quantum` for example `1514`. This is simplest way with 1Gbps fiber to provide QoS. More complex solution on the way...

I uploaded my current settings of QoS, it's working like that over 2 weeks and no issues at all even during heavy load of network. `Buckets=2048`, default value for OPNsense is `FQ-CoDel Flows=1024`. If you want to increase `FQ-CoDel Flows=2048` then also increase `Buckets=4096` so it's always double and avoid conflicts.

>[!IMPORTANT]
>Please remove in section "Queue"
>Target = 5
>Interval = 100
>It's totally enough if values are only in "Pipe"

## Agressive values
![alt text](https://github.com/nightcomdev/opnsense/blob/main/QoS/images/pipe.PNG "Pipe Download")

![alt text](https://github.com/nightcomdev/opnsense/blob/main/QoS/images/queue.PNG "Queue Download")

![alt text](https://github.com/nightcomdev/opnsense/blob/main/QoS/images/rules.PNG "Rules Download")

![alt text](https://github.com/nightcomdev/opnsense/blob/main/QoS/images/rules-upload.PNG "Rules Download")
## Conservative values




