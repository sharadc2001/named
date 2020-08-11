[root@ns1 ignition]# cat /var/named/data/whls.com.zone
$TTL 1W
@       IN      SOA     ns1.whls.com.   root (
                        2019070700      ; serial
                        3H              ; refresh (3 hours)
                        30M             ; retry (30 minutes)
                        2W              ; expiry (2 weeks)
                        1W )            ; minimum (1 week)
        IN      NS      ns1.whls.com.
        IN      MX 10   smtp.whls.com.
;
;
ns1     IN      A       9.98.30.44
smtp    IN      A       9.98.30.44
;
; The api points to the IP of your load balancer
api.ocp4                IN      A       9.98.30.59
api-int.ocp4            IN      A       9.98.30.59
;
; The wildcard also points to the load balancer
*.apps.ocp4             IN      A       9.98.30.59
;
; Create entry for the bootstrap host
bootstrap.ocp4  IN      A       9.98.30.45
;
; Create entries for the master hosts
master0.ocp4            IN      A       9.98.30.46
master1.ocp4            IN      A       9.98.30.47
master2.ocp4            IN      A       9.98.30.48
;
; Create entries for the worker hosts
worker0.ocp4            IN      A       9.98.30.54
worker1.ocp4            IN      A       9.98.30.55
worker2.ocp4            IN      A       9.98.30.56
;
; The ETCd cluster lives on the masters...so point these to the IP of the masters
etcd-0.ocp4     IN      A       9.98.30.46
etcd-1.ocp4     IN      A       9.98.30.47
etcd-2.ocp4     IN      A       9.98.30.48
;
; The SRV records are IMPORTANT....make sure you get these right...note the trailing dot at the end...
_etcd-server-ssl._tcp.ocp4.whls.com     IN      SRV     0 10 2380 etcd-0.ocp4.whls.com.
_etcd-server-ssl._tcp.ocp4.whls.com     IN      SRV     0 10 2380 etcd-1.ocp4.whls.com.
_etcd-server-ssl._tcp.ocp4.whls.com     IN      SRV     0 10 2380 etcd-2.ocp4.whls.com.
;
;EOF


[root@ns1 ignition]# cat /var/named/data/named.whls.zone
$TTL 1W
@       IN      SOA     ns1.whls.com.   root (
                        2019070700      ; serial
                        3H              ; refresh (3 hours)
                        30M             ; retry (30 minutes)
                        2W              ; expiry (2 weeks)
                        1W )            ; minimum (1 week)
        IN      NS      ns1.whls.com.
;
; syntax is "last octet" and the host must have fqdn with trailing dot
46      IN      PTR     master0.ocp4.whls.com.
47      IN      PTR     master1.ocp4.whls.com.
48      IN      PTR     master2.ocp4.whls.com.
;
45      IN      PTR     bootstrap.ocp4.whls.com.
;
59      IN      PTR     api.ocp4.whls.com.
59      IN      PTR     api-int.ocp4.whls.com.
;
54      IN      PTR     worker0.ocp4.whls.com.
55      IN      PTR     worker1.ocp4.whls.com.
56      IN      PTR     worker2.ocp4.whls.com.
;
;EOF
