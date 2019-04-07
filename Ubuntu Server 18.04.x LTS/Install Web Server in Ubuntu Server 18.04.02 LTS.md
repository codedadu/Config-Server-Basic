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
For Check the Status of Apache2 Web Server, you can execute command in the bellow:

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

## Bag. 2.1: Adjust firewall to allow Apache web server
By default, the apache web browser can’t be accessed from remote systems if you have enabled the UFW firewall in Ubuntu 18.04 LTS. 
You must allow the http and https traffic via UFW by following the below steps.

First, let us view which applications have installed a profile using command:
```
root@server:/home/nara# ufw app list
Available applications:
  Apache
  Apache Full
  Apache Secure
  OpenSSH
root@server:/home/nara# 
```
As you can see, Apache and OpenSSH applications have installed UFW profiles.

If you look into the “Apache Full” profile, you will see that it enables traffic to the ports 80 and 443:
```
root@server:/home/nara# ufw app info "Apache Full"
Profile: Apache Full
Title: Web Server (HTTP,HTTPS)
Description: Apache v2 is the next generation of the omnipresent Apache web
server.

Ports:
  80,443/tcp
root@server:/home/nara# 
```
Now, run the following command to allow incoming HTTP and HTTPS traffic for this profile:
```
root@server:/home/nara# ufw allow in "Apache Full"
Rules updated
Rules updated (v6)
root@server:/home/nara# 
```
If you don’t want to allow https traffic, but only http (80) traffic, run this command instead:
```
root@server:/home/nara# ufw app info "Apache"
Profile: Apache
Title: Web Server
Description: Apache v2 is the next generation of the omnipresent Apache web
server.

Port:
  80/tcp
root@server:/home/nara# 
```
Now, open up your web browser and navigate to http://localhost/ or http://IP-Address/.
If you are see a screen something like above, you are good to go. Apache server is working!

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/webserver-after-ufw.PNG"/>

## 3.1: Install MariaDB
MariaDB is the drop-in replacement of MySQL database server.
To install it, run:
```
root@server:/home/nara# apt -y install mariadb-server mariadb-client
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
0 upgraded, 30 newly installed, 0 to remove and 0 not upgraded.
Need to get 24.2 MB of archives.
After this operation, 184 MB of additional disk space will be used.
```
The version of MariaDB in the Ubuntu official repositories might be outdated. If you want to install a latest MariaDB, 
add the <a href="https://downloads.mariadb.org/mariadb/repositories/#mirror=Beritagar&distro=Ubuntu&distro_release=bionic--ubuntu_bionic&version=10.3">MariaDB official repository</a> for Ubuntu and install it as shown below.

```
Processing triggers for systemd (237-3ubuntu10.17) ...
Setting up libhtml-parser-perl (3.72-3build1) ...
Setting up libcgi-pm-perl (4.38-1) ...
Processing triggers for man-db (2.8.3-2ubuntu0.1) ...

Progress: [ 80%] [######################################################################....................] 
Setting up mariadb-server (1:10.1.38-0ubuntu0.18.04.1) ...
Processing triggers for libc-bin (2.27-3ubuntu1) ...
Processing triggers for systemd (237-3ubuntu10.17) ...
Processing triggers for ureadahead (0.100.0-20) ...
root@server:/home/nara# 
```
First, add MariaDB repository and import the key as shown below.

```
root@server:/home/nara# apt-get install software-properties-common
Reading package lists... Done
Building dependency tree       
Reading state information... Done
software-properties-common is already the newest version (0.96.24.32.7).
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
root@server:/home/nara#
```
Next
```
root@server:/home/nara# apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
Executing: /tmp/apt-key-gpghome.PYbkvRA2aG/gpg.1.sh --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
gpg: key F1656F24C74CD1D8: 6 signatures not checked due to missing keys
gpg: key F1656F24C74CD1D8: public key "MariaDB Signing Key <signing-key@mariadb.org>" imported
gpg: Total number processed: 1
gpg:               imported: 1
root@server:/home/nara# 
```
Next
```
root@server:/home/nara# add-apt-repository 'deb [arch=amd64,arm64,ppc64el] http://sgp1.mirrors.digitalocean.com/mariadb/repo/10.3/ubuntu bionic main'
Get:1 http://sgp1.mirrors.digitalocean.com/mariadb/repo/10.3/ubuntu bionic InRelease [3,887 B]
Hit:2 http://archive.ubuntu.com/ubuntu bionic InRelease
Get:3 http://archive.ubuntu.com/ubuntu bionic-updates InRelease [88.7 kB]
Get:4 http://sgp1.mirrors.digitalocean.com/mariadb/repo/10.3/ubuntu bionic/main amd64 Packages [7,992 B]
Get:5 http://archive.ubuntu.com/ubuntu bionic-backports InRelease [74.6 kB]                  
Get:6 http://sgp1.mirrors.digitalocean.com/mariadb/repo/10.3/ubuntu bionic/main ppc64el Packages [7,753 B]
Get:7 http://archive.ubuntu.com/ubuntu bionic-security InRelease [88.7 kB]    
Get:8 http://sgp1.mirrors.digitalocean.com/mariadb/repo/10.3/ubuntu bionic/main arm64 Packages [7,762 B]
Fetched 279 kB in 5s (53.9 kB/s)                           
Reading package lists... Done
root@server:/home/nara# 
```
After adding the repository, run the following commands to install MariaDB.

```
root@server:/home/nara# apt -y update
Hit:1 http://sgp1.mirrors.digitalocean.com/mariadb/repo/10.3/ubuntu bionic InRelease
Hit:2 http://archive.ubuntu.com/ubuntu bionic InRelease
Hit:3 http://archive.ubuntu.com/ubuntu bionic-updates InRelease
Hit:4 http://archive.ubuntu.com/ubuntu bionic-backports InRelease
Hit:5 http://archive.ubuntu.com/ubuntu bionic-security InRelease
Reading package lists... Done                      
Building dependency tree       
Reading state information... Done
5 packages can be upgraded. Run 'apt list --upgradable' to see them.
root@server:/home/nara#
```

Before Update the MySQL Server Version

```
root@server:/home/nara# mysql --version
mysql  Ver 15.1 Distrib 10.1.38-MariaDB, for debian-linux-gnu (x86_64) using readline 5.2
```

Update the MySQL Server

```
root@server:/home/nara# apt -y install mariadb-server
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages were automatically installed and are no longer required:
  libconfig-inifiles-perl libjemalloc1
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  mariadb-client mariadb-client-10.3 mariadb-client-core-10.3 mariadb-common mariadb-server-10.3
  mariadb-server-core-10.3
Suggested packages:
  mailx mariadb-test tinyca
The following packages will be REMOVED:
  mariadb-client-10.1 mariadb-client-core-10.1 mariadb-server-10.1 mariadb-server-core-10.1
The following NEW packages will be installed:
  mariadb-client-10.3 mariadb-client-core-10.3 mariadb-server-10.3 mariadb-server-core-10.3
The following packages will be upgraded:
  mariadb-client mariadb-common mariadb-server
3 upgraded, 4 newly installed, 4 to remove and 2 not upgraded.
Preparing to unpack .../mariadb-server_1%3a10.3.14+maria~bionic_all.deb ...
Unpacking mariadb-server (1:10.3.14+maria~bionic) over (1:10.1.38-0ubuntu0.18.04.1) ...
(Reading database ... 68251 files and directories currently installed.)
Removing mariadb-server-10.1 (1:10.1.38-0ubuntu0.18.04.1) ...
Removing mariadb-client-10.1 (1:10.1.38-0ubuntu0.18.04.1) ...

Progress: [ 21%] [####################.......................................................] 
```

Put the Password for MySQL

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/mysql-pass-1.PNG"/>

Next Verify the Password for MySQL 

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/mysql-pass-2.PNG"/>

Verify if MariaDB service is running or not using command:

```
root@server:/home/nara# /etc/init.d/mysql status
● mariadb.service - MariaDB 10.3.14 database server
   Loaded: loaded (/lib/systemd/system/mariadb.service; enabled; vendor preset: enabled)
  Drop-In: /etc/systemd/system/mariadb.service.d
           └─migrated-from-my.cnf-settings.conf
   Active: active (running) since Sun 2019-04-07 13:16:54 UTC; 48s ago
     Docs: man:mysqld(8)
           https://mariadb.com/kb/en/library/systemd/
 Main PID: 6494 (mysqld)
   Status: "Taking your SQL requests now..."
    Tasks: 31 (limit: 505)
   CGroup: /system.slice/mariadb.service
           └─6494 /usr/sbin/mysqld

root@server:/home/nara#
```

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/mysql-pass-3.PNG"/>

## 3.2: Setup database administrative user (root) password
During MariaDB installation, It will set password for the administrative user account (root).
If you try to setup password manually using command:
```
root@server:/home/nara# mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): 
ERROR 1524 (HY000): Plugin 'unix_socket' is not loaded
Enter current password for root (enter for none): 
ERROR 1524 (HY000): Plugin 'unix_socket' is not loaded
Enter current password for root (enter for none): 
```

after that open the new of tab in ssh remote

```
nara@server:~$ sudo su
[sudo] password for nara: 
root@server:/home/nara# /etc/init.d/mysql stop
[ ok ] Stopping mysql (via systemctl): mysql.service.
root@server:/home/nara# mysqld_safe --skip-grant-tables &
[1] 6854
root@server:/home/nara# 190407 13:31:25 mysqld_safe Logging to syslog.
190407 13:31:25 mysqld_safe Starting mysqld daemon with databases from /var/lib/mysql
```

and access MySQL in another tab with command in bellow

```
root@server:/home/nara# mysql -u root
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 8
Server version: 10.3.14-MariaDB-1:10.3.14+maria~bionic mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> 
```

and quering the bellow 

```
MariaDB [(none)]> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [mysql]> update user set password=PASSWORD("21091994") where User='root';
Query OK, 0 rows affected (0.001 sec)
Rows matched: 1  Changed: 0  Warnings: 0

MariaDB [mysql]> update user set plugin="mysql_native_password";
Query OK, 2 rows affected (0.001 sec)
Rows matched: 2  Changed: 2  Warnings: 0

MariaDB [mysql]> quit;
Bye
root@server:/home/nara# 
```

and process is success in the bellow

```
root@server:/home/nara# /etc/init.d/mysql stop
[ ok ] Stopping mysql (via systemctl): mysql.service.
root@server:/home/nara# kill -9 $(pgrep mysql)
root@server:/home/nara# /etc/init.d/mysql start
[ ok ] Starting mysql (via systemctl): mysql.service.
[1]+  Killed                  mysqld_safe --skip-grant-tables
root@server:/home/nara# mysql -u root -p
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 11
Server version: 10.3.14-MariaDB-1:10.3.14+maria~bionic mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> 
```

in the bellow of pictures access 

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/config-mysq-1.PNG"/>

and the bellow 

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/config-mysq-2.PNG"/>

if the process is successful as above, it means the process is running correctly

Link Source Solve the Problem in Here: <a href="https://askubuntu.com/questions/705458/ubuntu-15-10-mysql-error-1524-unix-socket"/>https://askubuntu.com/questions/705458/ubuntu-15-10-mysql-error-1524-unix-socket</a>

Now, you can set database administrative password using command:
Enter password password, and hit ENTER key to accept the default values.

```
root@server:/home/nara# mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

You already have a root password set, so you can safely answer 'n'.

Change the root password? [Y/n] n
 ... skipping.

By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
root@server:/home/nara# 
```

make sure the process looks like it appears the command that was executed above and succeeded
That’s it. Password for the database administrative user account has been set.

```
root@server:/home/nara# mysql -u root -p
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 18
Server version: 10.3.14-MariaDB-1:10.3.14+maria~bionic mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
3 rows in set (0.001 sec)

MariaDB [(none)]> quit;
Bye
root@server:/home/nara# mysql --version
mysql  Ver 15.1 Distrib 10.3.14-MariaDB, for debian-linux-gnu (x86_64) using readline 5.2
root@server:/home/nara# 
```

Done.

