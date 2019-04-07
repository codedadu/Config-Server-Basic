# Install Apache, MariaDB, PHP (LAMP stack) in Ubuntu 18.04 LTS Server
For the purpose of this tutorial, I will be using the following testbox.

- Operating System : Ubuntu 18.04.02 64 bit LTS server
- IP address : 192.168.43.50/24

## Bag. 1.1: Install Apache webserver
To install Apache web server, run the following command from the Terminal:

```
nara@server:~$ sudo su
[sudo] password for nara: 
root@server:/home/nara# 

root@server:/home/nara# apt -y install apache2
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  apache2-bin apache2-data apache2-utils libapr1 libaprutil1 libaprutil1-dbd-sqlite3 libaprutil1-ldap liblua5.2-0
  ssl-cert
Suggested packages:
  www-browser apache2-doc apache2-suexec-pristine | apache2-suexec-custom openssl-blacklist
The following NEW packages will be installed:
  apache2 apache2-bin apache2-data apache2-utils libapr1 libaprutil1 libaprutil1-dbd-sqlite3 libaprutil1-ldap
  liblua5.2-0 ssl-cert
0 upgraded, 10 newly installed, 0 to remove and 0 not upgraded.
Need to get 1,730 kB of archives.
After this operation, 6,985 kB of additional disk space will be used.
Get:1 http://archive.ubuntu.com/ubuntu bionic/main amd64 libapr1 amd64 1.6.3-2 [90.9 kB]
Get:2 http://archive.ubuntu.com/ubuntu bionic/main amd64 libaprutil1 amd64 1.6.1-2 [84.4 kB]
Processing triggers for systemd (237-3ubuntu10.17) ...
Processing triggers for ureadahead (0.100.0-20) ...
Processing triggers for ufw (0.36-0ubuntu0.18.04.1) ...
Progress: [ 98%] [########################################################################################..]
checlean.service.
Processing triggers for libc-bin (2.27-3ubuntu1) ...
Processing triggers for systemd (237-3ubuntu10.17) ...
Processing triggers for ureadahead (0.100.0-20) ...
Processing triggers for ufw (0.36-0ubuntu0.18.04.1) ...
root@server:/home/nara# 
```

## Bag. 1.2: See the Status of Apache2

```
root@server:/home/nara# /etc/init.d/apache2 status
● apache2.service - The Apache HTTP Server
   Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
  Drop-In: /lib/systemd/system/apache2.service.d
           └─apache2-systemd.conf
   Active: active (running) since Sun 2019-04-07 12:25:16 UTC; 1min 11s ago
 Main PID: 2456 (apache2)
    Tasks: 55 (limit: 505)
   CGroup: /system.slice/apache2.service
           ├─2456 /usr/sbin/apache2 -k start
           ├─2597 /usr/sbin/apache2 -k start
           └─2598 /usr/sbin/apache2 -k start

Apr 07 12:25:06 server systemd[1]: Starting The Apache HTTP Server...
Apr 07 12:25:16 server apachectl[2435]: AH00558: apache2: Could not reliably determine the server's 
fully qualif… messageApr 07 12:25:16 server systemd[1]: Started The Apache HTTP Server.
Hint: Some lines were ellipsized, use -l to show in full.
root@server:/home/nara# 
```
or
```
root@server:/home/nara# systemctl status apache2
● apache2.service - The Apache HTTP Server
   Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
  Drop-In: /lib/systemd/system/apache2.service.d
           └─apache2-systemd.conf
   Active: active (running) since Sun 2019-04-07 12:25:16 UTC; 2min 31s ago
 Main PID: 2456 (apache2)
    Tasks: 55 (limit: 505)
   CGroup: /system.slice/apache2.service
           ├─2456 /usr/sbin/apache2 -k start
           ├─2597 /usr/sbin/apache2 -k start
           └─2598 /usr/sbin/apache2 -k start

Apr 07 12:25:06 server systemd[1]: Starting The Apache HTTP Server...
Apr 07 12:25:16 server apachectl[2435]: AH00558: apache2: Could not reliably determine the server's 
fully qualified domaiApr 07 12:25:16 server systemd[1]: Started The Apache HTTP Server.
```

## Bag. 1.3: Ceck & Access the Web Server in Web Browser Client
First, ceck the IP Address in your VM with command the bellow:
```
root@server:/home/nara# ifconfig 
enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.43.50  netmask 255.255.255.0  broadcast 192.168.43.255
        inet6 fe80::a00:27ff:fe67:cee2  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:67:ce:e2  txqueuelen 1000  (Ethernet)
        RX packets 60424  bytes 50839940 (50.8 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 18594  bytes 1619245 (1.6 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
and open in your Web Browser Client with IP Address VM

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/webserver.PNG"/>