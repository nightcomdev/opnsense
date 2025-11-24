# Unbound performance optimization 

Content will be added soon...be patient 

Create file `/usr/local/etc/unbound.opnsense.d/99-custom.conf`  s

`server:
    ###############################################
    # SOCKET BUFFERS – niski jitter, szybkie DNS
    ###############################################
    so-rcvbuf: 4m
    so-sndbuf: 4m
    so-reuseport: yes

    ###############################################
    # CACHE – duży, stabilny, wydajny
    ###############################################
    msg-cache-size: 128m
    rrset-cache-size: 256m
    neg-cache-size: 10m

    ###############################################
    # PREFETCH – przyspiesza ładowanie stron
    ###############################################
    prefetch: yes
    prefetch-key: yes

    ###############################################
    # QUERY / THREAD TUNING
    ###############################################
    num-queries-per-thread: 2048
    outgoing-range: 4096

    ###############################################
    # SERVE EXPIRED – odporność na przerwy w internecie
    ###############################################
    serve-expired: yes
    serve-expired-ttl: 86400
    serve-expired-reply-ttl: 30

    ###############################################
    # DNS STABILITY
    ###############################################
    infra-cache-numhosts: 20000
    infra-host-ttl: 900
    `
