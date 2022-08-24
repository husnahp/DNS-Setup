# DNS-Setup
root@saturn:~# cd /etc/bind
root@saturn:/etc/bind# systemctl status named
● named.service - BIND Domain Name Server
     Loaded: loaded (/lib/systemd/system/named.service; enabled; vendor preset:>
     Active: active (running) since Wed 2022-08-24 15:40:40 UTC; 3min 46s ago
       Docs: man:named(8)
    Process: 1519 ExecStart=/usr/sbin/named $OPTIONS (code=exited, status=0/SUC>
   Main PID: 1520 (named)
      Tasks: 8 (limit: 2241)
     Memory: 7.9M
        CPU: 58ms
     CGroup: /system.slice/named.service
             └─1520 /usr/sbin/named -u bind

Aug 24 15:40:40 saturn named[1520]: network unreachable resolving './DNSKEY/IN'>
Aug 24 15:40:40 saturn named[1520]: network unreachable resolving './NS/IN': 20>
Aug 24 15:40:40 saturn named[1520]: network unreachable resolving './DNSKEY/IN'>
Aug 24 15:40:40 saturn named[1520]: network unreachable resolving './NS/IN': 20>
Aug 24 15:40:40 saturn named[1520]: network unreachable resolving './DNSKEY/IN'>
Aug 24 15:40:40 saturn named[1520]: network unreachable resolving './NS/IN': 20>
Aug 24 15:40:40 saturn named[1520]: network unreachable resolving './DNSKEY/IN'>
Aug 24 15:40:40 saturn named[1520]: network unreachable resolving './NS/IN': 20>
Aug 24 15:40:40 saturn named[1520]: managed-keys-zone: Initializing automatic t>
Aug 24 15:40:40 saturn named[1520]: resolver priming query complete: success

root@saturn:/etc/bind# systemctl stop named
root@saturn:/etc/bind# systemctl disable named
Synchronizing state of named.service with SysV service script with /lib/systemd/                                                                                                             systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install disable named
Removed /etc/systemd/system/bind9.service.
Removed /etc/systemd/system/multi-user.target.wants/named.service.
root@saturn:/etc/bind# systemctl status system-resolved.service
Unit system-resolved.service could not be found.
root@saturn:/etc/bind# vi /etc/resolv.conf
root@saturn:/etc/bind# cat /etc/resolv.conf
nameserver 8.8.8.8
nameserver 192.168.0.115

root@saturn:/etc/bind# vi named.conf.local
root@saturn:/etc/bind# cat named.conf.local
//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";
zone "example.com" {
        type master;
        file "/etc/bind/example.com";
};
root@saturn:/etc/bind# vi example.com
root@saturn:/etc/bind# cat example.com
@   IN  SOA example.com. root.example.com. (
                  2     ; Serial
             604800     ; Refresh
              86400     ; Retry
            2419200     ; Expire
             604800 )   ; Negative Cache TTL
;
@   IN  NS  example.com.
@   IN  A   192.168.0.115
husnaHP   IN   A   192.168.0.115
root@saturn:/etc/bind# systemctl start named
root@saturn:/etc/bind# systemctl status named
● named.service - BIND Domain Name Server
     Loaded: loaded (/lib/systemd/system/named.service; disabled; vendor preset>
     Active: active (running) since Wed 2022-08-24 15:55:21 UTC; 7s ago
       Docs: man:named(8)
    Process: 2597 ExecStart=/usr/sbin/named $OPTIONS (code=exited, status=0/SUC>
   Main PID: 2598 (named)
      Tasks: 5 (limit: 2241)
     Memory: 6.0M
        CPU: 234ms
     CGroup: /system.slice/named.service
             └─2598 /usr/sbin/named -u bind

Aug 24 15:55:21 saturn named[2598]: network unreachable resolving './NS/IN': 20>
Aug 24 15:55:21 saturn named[2598]: network unreachable resolving './DNSKEY/IN'>
Aug 24 15:55:21 saturn named[2598]: network unreachable resolving './NS/IN': 20>
Aug 24 15:55:21 saturn systemd[1]: Started BIND Domain Name Server.
Aug 24 15:55:21 saturn named[2598]: network unreachable resolving './DNSKEY/IN'>
Aug 24 15:55:21 saturn named[2598]: network unreachable resolving './NS/IN': 20>
Aug 24 15:55:21 saturn named[2598]: network unreachable resolving './DNSKEY/IN'>
Aug 24 15:55:21 saturn named[2598]: network unreachable resolving './NS/IN': 20>
Aug 24 15:55:21 saturn named[2598]: managed-keys-zone: Key 20326 for zone . is >
Aug 24 15:55:21 saturn named[2598]: resolver priming query complete: success
lines 1-22/22 (END)
^C
root@saturn:/etc/bind# nslookup example.com 192.168.0.115
Server:         192.168.0.115
Address:        192.168.0.115#53

Name:   example.com
Address: 192.168.0.115

root@saturn:/etc/bind# nslookup husnaHP.trainees.com 192.168.0.115
Server:         192.168.0.115
Address:        192.168.0.115#53

Non-authoritative answer:
Name:   husnaHP.trainees.com
Address: 103.224.182.245
