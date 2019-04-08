# Install phpMyAdmin in Ubuntu Server 18.04.02 LTS
Pertama, ketik command dibawah ini untuk menginstallkan phpMyAdmin di Server Ubuntu 18.04.02 LTS Kita
dengan command dibawah ini

```
root@server:/home/nara# apt -y install phpmyadmin
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages were automatically installed and are no longer required:
  libconfig-inifiles-perl libjemalloc1
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  dbconfig-common dbconfig-mysql fontconfig-config fonts-dejavu-core javascript-common libapache2-mod-php7.2 libfontconfig1 libgd3 libjbig0
  libjpeg-turbo8 libjpeg8 libjs-jquery libjs-sphinxdoc libjs-underscore libsodium23 libtiff5 libwebp6 libxpm4 libzip4 php php-bz2 php-common
  php-curl php-gd php-mbstring php-mysql php-pear php-php-gettext php-phpseclib php-tcpdf php-xml php-zip php7.2 php7.2-bz2 php7.2-cli
  php7.2-common php7.2-curl php7.2-gd php7.2-json php7.2-mbstring php7.2-mysql php7.2-opcache php7.2-readline php7.2-xml php7.2-zip
Suggested packages:
  libgd-tools php-libsodium php-mcrypt php-gmp php-imagick www-browser
The following NEW packages will be installed:
  dbconfig-common dbconfig-mysql fontconfig-config fonts-dejavu-core javascript-common libapache2-mod-php7.2 libfontconfig1 libgd3 libjbig0
  libjpeg-turbo8 libjpeg8 libjs-jquery libjs-sphinxdoc libjs-underscore libsodium23 libtiff5 libwebp6 libxpm4 libzip4 php php-bz2 php-common
  php-curl php-gd php-mbstring php-mysql php-pear php-php-gettext php-phpseclib php-tcpdf php-xml php-zip php7.2 php7.2-bz2 php7.2-cli
  php7.2-common php7.2-curl php7.2-gd php7.2-json php7.2-mbstring php7.2-mysql php7.2-opcache php7.2-readline php7.2-xml php7.2-zip
  phpmyadmin
0 upgraded, 46 newly installed, 0 to remove and 2 not upgraded.
Need to get 19.7 MB of archives.
```

Jika ditengah-tengah installasi terjadi error pada saat konfig seperti gambar dibawah ini

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/phpmyadmin-1.PNG"/>

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/phpmyadmin-2.PNG"/>

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/phpmyadmin-3.PNG"/>

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/phpmyadmin-4.PNG"/>

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/phpmyadmin-5.PNG"/>

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/phpmyadmin-6.PNG"/>

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/phpmyadmin-7.PNG"/>

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/phpmyadmin-8.PNG"/>

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/phpmyadmin-9.PNG"/>

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/phpmyadmin-10.PNG"/>

ketik command berikut untuk meresolve masalah diatas

```
root@server:/home/nara# nano /etc/phpmyadmin/config.inc.php
```

cari pada baris bagian ini dan rubah seperti tampak pada gambar dibawah ini


<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/phpmyadmin-11.PNG"/>


simpan kemudian restart apache2 dan reload phpmyadminnya

```
root@server:/home/nara# /etc/init.d/apache2 restart
[ ok ] Restarting apache2 (via systemctl): apache2.service.
root@server:/home/nara# 
```

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/phpmyadmin-12.PNG"/>

dan akan tersisa error seperti dibawah ini, klik pada bagian biru seperti dibawah untuk meresolvenya

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/phpmyadmin-13.PNG"/>

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/phpmyadmin-14.PNG"/>

dan berikut adalah tampilan dari hasil resolve diatas, Done.

<img src="https://github.com/codedadu/Linux-Bash-Config/blob/master/Ubuntu%20Server%2018.04.x%20LTS/captures/phpmyadmin-15.PNG"/>

Sumber Resolve: <a href="https://github.com/septiyadii/Issue-Configuration/issues/4">https://github.com/septiyadii/Issue-Configuration/issues/4</a>