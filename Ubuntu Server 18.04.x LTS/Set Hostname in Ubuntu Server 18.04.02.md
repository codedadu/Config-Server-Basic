# How to Change Hostname on Ubuntu 18.04
This tutorial will guide you through the process of changing the hostname on an Ubuntu 18.04 system.
The hostname is set at the time when the Ubuntu operating system is installed or if you are spinning up 
a virtual machine it is dynamically assigned to the instance at startup.
The method described in this guide will work without the need of restarting your system.
Although this tutorial is written for Ubuntu 18.04 the same instructions apply for Ubuntu 16.04 and any 
Ubuntu-based distribution, including Linux Mint and Elementary OS.

## Bag. 1: Display the Current Hostname
To view the current hostname, enter the following command:

```
nara@server:~$ sudo su
[sudo] password for nara: 
root@server:/home/nara# hostnamectl
   Static hostname: server
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 5397a09c752846b493048758ab55ec62
           Boot ID: 90ab9371850c435bae9cdfe1f4038343
    Virtualization: oracle
  Operating System: Ubuntu 18.04.2 LTS
            Kernel: Linux 4.15.0-47-generic
      Architecture: x86-64
root@server:/home/nara# 
```

As you can see in the image above, the current hostname is set to server.

## Bag. 2: Change the Hostname
The following steps outline how to change the hostname in Ubuntu 18.04
Change the hostname using hostnamectl.
In Ubuntu 18.04 we can change the system hostname and related settings using the command hostnamectl.
For example, to change the system static hostname to linuxize, you would use the following command:

```
root@server:/home/nara# hostnamectl set-hostname webserver
root@server:/home/nara# 
```

The hostnamectl command does not produce output. On success, 0 is returned, a non-zero failure code otherwise.

## Bag. 3: Edit the /etc/hosts file.
Open the /etc/hosts file and change the old hostname to the new one.

```
root@server:/home/nara# cp /etc/hosts /etc/hosts.backup
root@server:/home/nara# nano /etc/hosts
```

Next

```
127.0.0.1   localhost
127.0.0.1   webserver

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

## Bag. 4: Edit the cloud.cfg file.
If the cloud-init package is installed you also need to edit the cloud.cfg file. This package is usually installed by
default in the images provided by the cloud providers such as AWS and it is used to handle the initialization 
of the cloud instances.
To check if the package is installed run the following ls command, If the package is installed the output will look 
like the following:

```
root@server:/home/nara# ls -l /etc/cloud/cloud.cfg
-rw-r--r-- 1 root root 3594 Mar 11 21:07 /etc/cloud/cloud.cfg
root@server:/home/nara# 
```

In this case you’ll need to open the /etc/cloud/cloud.cfg file:

```
root@server:/home/nara# cp /etc/cloud/cloud.cfg /etc/cloud/cloud.cfg.backup
root@server:/home/nara# nano /etc/cloud/cloud.cfg
```

Next, Search for preserve_hostname and change the value from false to true:

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/barfore-setup-cloud.PNG"/>

```

# The top level settings are used as module
# and system configuration.

# A set of users which may be applied and/or used by various modules
# when a 'default' entry is found it will reference the 'default_user'
# from the distro configuration specified below
users:
   - default

# If this is set, 'root' will not be able to ssh in and they
# will get a message to login instead as the default $user
disable_root: true

# This will cause the set+update hostname module to not operate (if true)
preserve_hostname: true
```

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/after-setup-cloud.PNG"/>

Save the file and close your editor.

## Bag. 5: Verify the change
To verify that the hostname was successfully changed, once again use the hostnamectl command:

```
root@server:/home/nara# hostnamectl
   Static hostname: webserver
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 5397a09c752846b493048758ab55ec62
           Boot ID: 90ab9371850c435bae9cdfe1f4038343
    Virtualization: oracle
  Operating System: Ubuntu 18.04.2 LTS
            Kernel: Linux 4.15.0-47-generic
      Architecture: x86-64
root@server:/home/nara#
```

Done. & Reboot now the VM

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/reboot.PNG"/>

Sumber: <a href="https://linuxize.com/post/how-to-change-hostname-on-ubuntu-18-04/">https://linuxize.com/post/how-to-change-hostname-on-ubuntu-18-04/"/>