# HA proxy

Dans **/etc/haproxy/haproxy.cfg**

```config
#listen web-cluster
#    bind 0.0.0.0:8080
#    mode http
    # Activation des statistiques
#    stats enable
#    stats uri /stats
#    stats realm Strictly\ Private
#    stats auth admin:admin
    # Équilibrage de charge
#    balance roundrobin
#    option httpclose
#    option forwardfor
    # Serveurs backend
#    server http1 192.168.28.40:80 check
#    server http2 192.168.28.41:80 check



frontend mysql-frontend
    bind *:3306
    mode tcp
    default_backend mysql-backend

backend mysql-backend
    mode tcp
    balance roundrobin
    server mysql1 192.168.128.20:3306 check
    server mysql2 192.168.128.21:3306 check
```