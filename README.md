# cneir

multi website

# prepare vps

## buy cheapest TencentCloud VPS 65 - 30 = 35 RMB 

	1 Core, 1GB RAM , 20GB HDD, Ubuntu 14.04.LTS

## login by ssh client use default user ubuntu 

	ubuntu@VM-182-32-ubuntu:~$

### hardware info

* cpu

		cat /proc/cpuinfo

		processor       : 0
		vendor_id       : GenuineIntel
		cpu family      : 6
		model           : 62
		model name      : Intel(R) Xeon(R) CPU E5-26xx v2
		stepping        : 4
		microcode       : 0x1
		cpu MHz         : 2593.748
		cache size      : 4096 KB
		physical id     : 0
		siblings        : 1
		core id         : 0
		cpu cores       : 1
		apicid          : 0
		initial apicid  : 0
		fpu             : yes
		fpu_exception   : yes
		cpuid level     : 13
		wp              : yes
		flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx lm constant_tsc rep_good nopl eagerfpu pni pclmulqdq ssse3 cx16 pcid sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm xsaveopt
		bogomips        : 5187.49
		clflush size    : 64
		cache_alignment : 64
		address sizes   : 40 bits physical, 48 bits virtual
		power management:

* harddisk

		df -h

		Filesystem      Size  Used Avail Use% Mounted on
		/dev/vda1        20G  1.5G   18G   8% /
		none            4.0K     0  4.0K   0% /sys/fs/cgroup
		udev            493M  4.0K  493M   1% /dev
		tmpfs           100M  308K  100M   1% /run
		none            5.0M     0  5.0M   0% /run/lock
		none            498M   24K  497M   1% /run/shm
		none            100M     0  100M   0% /run/user

* memory

		cat /proc/meminfo

		MemTotal:        1017896 kB
		MemFree:          554680 kB
		Buffers:           37392 kB
		Cached:           330440 kB
		SwapCached:            0 kB
		Active:           179272 kB
		Inactive:         243664 kB
		Active(anon):      55172 kB
		Inactive(anon):      276 kB
		Active(file):     124100 kB
		Inactive(file):   243388 kB
		Unevictable:           0 kB
		Mlocked:               0 kB
		SwapTotal:             0 kB
		SwapFree:              0 kB
		Dirty:                 0 kB
		Writeback:             0 kB
		AnonPages:         55100 kB
		Mapped:             9000 kB
		Shmem:               348 kB
		Slab:              26292 kB
		SReclaimable:      18452 kB
		SUnreclaim:         7840 kB
		KernelStack:         640 kB
		PageTables:         3144 kB
		NFS_Unstable:          0 kB
		Bounce:                0 kB
		WritebackTmp:          0 kB
		CommitLimit:      508948 kB
		Committed_AS:     158724 kB
		VmallocTotal:   34359738367 kB
		VmallocUsed:        2092 kB
		VmallocChunk:   34359731783 kB
		HardwareCorrupted:     0 kB
		AnonHugePages:     12288 kB
		HugePages_Total:       0
		HugePages_Free:        0
		HugePages_Rsvd:        0
		HugePages_Surp:        0
		Hugepagesize:       2048 kB
		DirectMap4k:       26616 kB
		DirectMap2M:     1021952 kB

* os

		cat /etc/lsb-release

		DISTRIB_ID=Ubuntu
		DISTRIB_RELEASE=14.04
		DISTRIB_CODENAME=trusty
		DISTRIB_DESCRIPTION="Ubuntu 14.04.1 LTS"

* x86 or x64
		
		uname --m

		x86_64

* default users

		awk -F':' '{print $1}' /etc/passwd

		root
		daemon
		bin
		sys
		sync
		games
		man
		lp
		mail
		news
		uucp
		proxy
		www-data
		backup
		list
		irc
		gnats
		nobody
		libuuid
		syslog
		messagebus
		landscape
		ntp
		sshd
		ubuntu

# prepare user

## set root user passwords

	sudo passwd root

## allow root login as ssh

	su - root
	vim /etc/ssh/sshd_config

	# Press i to enter vim Insert Mode

	# comment PermitRootLogin without-password
		#PermitRootLogin without-password
	# input PermitRootLogin yes
		PermitRootLogin yes

	# Press Esc Quit vim Insert Mode, and input :wq to save and quit vim
	
	sudo service ssh restart

	# re login in ssh use account root

		Welcome to Ubuntu 14.04.1 LTS (GNU/Linux 3.13.0-36-generic x86_64)

		 * Documentation:  https://help.ubuntu.com/

		  System information as of Fri Jul 22 13:38:52 CST 2016

		  System load:  0.01              Processes:           74
		  Usage of /:   7.3% of 19.56GB   Users logged in:     0
		  Memory usage: 10%               IP address for eth0: ip.of.server.inner
		  Swap usage:   0%

		  Graph this data and manage this system at:
			https://landscape.canonical.com/

		The programs included with the Ubuntu system are free software;
		the exact distribution terms for each program are described in the
		individual files in /usr/share/doc/*/copyright.

		Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
		applicable law.

		Last login: Thu Jan  1 08:00:10 1970
		root@VM-182-32-ubuntu:~#

## change ssh server default listen port for security

1. add new_port to ssh 

		vim /etc/ssh/sshd_config

		# Press i to enter vim Insert Mode

		# add new_port under Port 22
		Port 22
		Port new_port

		# Press Esc Quit vim Insert Mode, and input :wq to save and quit vim

		sudo service ssh restart

		# add new rule on qcloud console web

			rules in

			TCP				new_port		0.0.0.0/0	allow	new_port

		# login suceess


2. remove old port 22 to ssh 

		# delete old 22 rule on qcloud console web

			rules in

			TCP				22		0.0.0.0/0	allow	22

		vim /etc/ssh/sshd_config

		# Press i to enter vim Insert Mode

		# comment Port 22 in config
		#Port 22
		Port new_port

		# Press Esc Quit vim Insert Mode, and input :wq to save and quit vim

		sudo service ssh restart
		
		# relogin on port 22 will fail

## update system from default source tencentyun ubuntu 14.04.LTS 

	sudo apt-get update
	sudo apt-get upgrade
	cat /etc/lsb-release
		DISTRIB_ID=Ubuntu
		DISTRIB_RELEASE=14.04
		DISTRIB_CODENAME=trusty
		DISTRIB_DESCRIPTION="Ubuntu 14.04.4 LTS"
	sudo apt-get dist-upgrade
	cat /etc/lsb-release
		DISTRIB_ID=Ubuntu
		DISTRIB_RELEASE=14.04
		DISTRIB_CODENAME=trusty
		DISTRIB_DESCRIPTION="Ubuntu 14.04.4 LTS"
	sudo reboot

## change ubuntu apt source to tsinghua ubuntu 16.04.LTS

	cp /etc/apt/sources.list /etc/apt/sources.list.backup
	cat /etc/apt/sources.list
	# show default source tencentyun ubuntu 14.04.LTS

	deb http://mirrors.tencentyun.com/ubuntu trusty main restricted universe multiverse
	deb http://mirrors.tencentyun.com/ubuntu trusty-updates main restricted universe multiverse
	deb http://mirrors.tencentyun.com/ubuntu-security trusty-security main
	deb-src http://mirrors.tencentyun.com/ubuntu trusty main restricted universe multiverse
	deb-src http://mirrors.tencentyun.com/ubuntu trusty-updates main restricted universe multiverse

	vim /etc/apt/sources.list

	# replace all content with follows
	# Press i to enter vim Insert Mode

	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main multiverse restricted universe
	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main multiverse restricted universe
	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-proposed main multiverse restricted universe
	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main multiverse restricted universe
	deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main multiverse restricted universe
	deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main multiverse restricted universe
	deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main multiverse restricted universe
	deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-proposed main multiverse restricted universe
	deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main multiverse restricted universe
	deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main multiverse restricted universe

	# Press Esc Quit vim Insert Mode, and input :wq to save and quit vim


	# add tsinghua ubuntu apt source on qcloud console web

	rules out

		TCP				80		166.111.206.63	allow	tsinghua ubuntu apt source


	apt-get update
	apt-get autoremove

	cat /etc/lsb-release
		DISTRIB_ID=Ubuntu
		DISTRIB_RELEASE=14.04
		DISTRIB_CODENAME=trusty
		DISTRIB_DESCRIPTION="Ubuntu 14.04.4 LTS"

	apt-get -y upgrade

	cat /etc/lsb-release
		DISTRIB_ID=Ubuntu
		DISTRIB_RELEASE=16.04
		DISTRIB_CODENAME=xenial
		DISTRIB_DESCRIPTION="Ubuntu 16.04.1 LTS"

	apt-get autoremove
	apt-get dist-upgrade
	apt-get autoremove
	apt-get update
	apt-get upgrade
		0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
	init 6 # reboot
	cat /etc/lsb-release
		DISTRIB_ID=Ubuntu
		DISTRIB_RELEASE=16.04
		DISTRIB_CODENAME=xenial
		DISTRIB_DESCRIPTION="Ubuntu 16.04.1 LTS"


# framework

## http reverse proxy - nginx


	adduser nginx
    gpasswd -a nginx sudo
    su - nginx
    sudo apt-get update
    sudo apt-get install nginx
    Ctrl + D
	curl 127.0.0.1
	# will show these
		<!DOCTYPE html>
		<html>
		<head>
		<title>Welcome to nginx!</title>
		<style>
			body {
				width: 35em;
				margin: 0 auto;
				font-family: Tahoma, Verdana, Arial, sans-serif;
			}
		</style>
		</head>
		<body>
		<h1>Welcome to nginx!</h1>
		<p>If you see this page, the nginx web server is successfully installed and
		working. Further configuration is required.</p>

		<p>For online documentation and support please refer to
		<a href="http://nginx.org/">nginx.org</a>.<br/>
		Commercial support is available at
		<a href="http://nginx.com/">nginx.com</a>.</p>

		<p><em>Thank you for using nginx.</em></p>
		</body>
		</html>

## http web server - node.js

	adduser node
    gpasswd -a node sudo
    su - node

	# add nodesource rule on qcloud console web

		rules out

		TCP				80		192.241.233.42	allow		deb.nodesource.com
		TCP				80		151.101.76.162	allow		registry.npmjs.org:80
		TCP				443		151.101.76.162	allow		registry.npmjs.org:443

	sudo curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
	sudo apt-get install -y nodejs
	node -v
	# v4.4.7
	sudo npm update npm -g
	npm -v
	# 3.10.5
	sudo npm update npm -g
	npm -v
	# 3.10.6
    Ctrl + D

## git client

	sudo apt-get install git
	git --version
	# git version 2.7.4

## process manager for Node.js applications - pm2

	# add nodesource rule on qcloud console web

		rules out

		TCP                80        212.83.163.168    allow        ikt.pm2.io:80

    su - node
	sudo npm install pm2 -g

	npm WARN deprecated minimatch@2.0.10: Please update to minimatch 3.0.2 or higher to avoid a RegExp DoS issue

	sudo pm2 install pm2-logrotate
    Ctrl + D

## npm registery switcher nrm

	# add nodesource rule on qcloud console web

		rules out

		TCP			80			47.88.189.193    	allow        r.cnpmjs.org
		TCP			443			114.55.80.225		allow        registry.npm.taobao.org
		TCP			443			173.192.57.106		allow        registry.nodejitsu.com
		TCP			80			202.202.43.41		allow        registry.mirror.cqupt.edu.cn
		TCP			443			52.23.180.84		allow        skimdb.npmjs.com
		TCP			443			202.114.196.252		allow        registry.enpmjs.org
	
    su - node
	sudo npm install -g nrm
    Ctrl + D

## switch npm registery from default to taobao

	su - node
	sudo nrm use taobao
    Ctrl + D

## web app

### cneir on 127.0.0.1:40001
	
	su - node
	cd /home/node
	mkdir app
	cd /home/node/app
	mkdir cneir
	cd cneir
	sudo npm init
		https://github.com/remembercode/cneir.git
	vim app.js
	
	# Press i to enter vim Insert Mode
	# paste these flows

	var express = require('express');
	var app = express();

	app.get('/', function (req, res) {
	  res.send('Hello World, cneir!');
	});

	app.listen(40001, '127.0.0.1', function () {
	  console.log('cneir app listening on port 40001!');
	});

	# Press Esc Quit vim Insert Mode, and input :wq to save and quit vim

	sudo npm install express --save

	# after install pm2
	sudo pm2 start /home/node/app/cneir/app.js --name="cneir" --watch

	# curl 127.0.0.1:40001 will show : Hello World, cneir!
	# and WebBrowser can not access it with http://ip:40001
    Ctrl + D

### remembercode on 127.0.0.1:40002
	
	su - node
	cd /home/node/app
	mkdir remembercode
	cd remembercode
	npm init
		https://github.com/remembercode/remembercode.git
	vim app.js
	
	# Press i to enter vim Insert Mode
	# paste these flows

	var express = require('express');
	var app = express();

	app.get('/', function (req, res) {
	  res.send('Hello World, remembercode!');
	});

	app.listen(40002, '127.0.0.1', function () {
	  console.log('remembercode app listening on port 40002!');
	});

	# Press Esc Quit vim Insert Mode, and input :wq to save and quit vim

	sudo npm install express --save

	# after install pm2
	sudo pm2 start /home/node/app/remembercode/app.js --name="remembercode" --watch

	# curl 127.0.0.1:40002 will show : Hello World, remembercode!
	# and WebBrowser can not access it with http://www.cneir.net:40002
    Ctrl + D

### xsowe on 127.0.0.1:40003
	
	su - node
	cd /home/node/app
	mkdir xsowe
	cd xsowe
	npm init
		https://github.com/remembercode/xsowe.git
	vim app.js
	
	# Press i to enter vim Insert Mode
	# paste these flows

	var express = require('express');
	var app = express();

	app.get('/', function (req, res) {
	  res.send('Hello World, xsowe!');
	});

	app.listen(40003, '127.0.0.1', function () {
	  console.log('xsowe app listening on port 40003!');
	});

	# Press Esc Quit vim Insert Mode, and input :wq to save and quit vim

	sudo npm install express --save

	# after install pm2
	sudo pm2 start /home/node/app/xsowe/app.js --name="xsowe" --watch

	# curl 127.0.0.1:40003 will show : Hello World, xsowe!
	# and WebBrowser can not access it with http://www.cneir.net:40003
    Ctrl + D
	
### after all app running

	su - node
	sudo pm2 start /home/node/app/cneir/app.js --name="cneir" --watch
	sudo pm2 start /home/node/app/remembercode/app.js --name="remembercode" --watch
	sudo pm2 start /home/node/app/xsowe/app.js --name="xsowe" --watch
	sudo pm2 save
	sudo pm2 startup ubuntu
	#show : su -c "chmod +x /etc/init.d/pm2-init.sh && update-rc.d pm2-init.sh defaults"
	su -c "chmod +x /etc/init.d/pm2-init.sh && update-rc.d pm2-init.sh defaults"
	Ctrl + D
	reboot
	curl 127.0.0.1:40001 # show ...cneir...

### config nginx reverse config

	


## ~~edit default firewall rules group on qcloud console web~~

	rules in

		TCP				22		0.0.0.0/0	allow	22
		TCP				80		0.0.0.0/0	allow	80
		TCP				443		0.0.0.0/0	allow	443
		All traffic		ALL		0.0.0.0/0	deny	-

	rules out

		TCP				80		166.111.206.63	allow	tsinghua ubuntu apt source
		TCP				443		192.241.233.42	allow	deb.nodesource.com
		TCP				443		151.101.76.162	allow	registry.npmjs.org:443
		TCP				80		151.101.76.162	allow	registry.npmjs.org:80
		TCP				80		212.83.163.168	allow	ikt.pm2.io:80
		All traffic		ALL		0.0.0.0/0		deny

	curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
	sudo apt-get install -y nodejs
	node -v
	# v4.4.7
	npm update npm -g
	npm -v
	# 3.10.6



## database

### CRUD framework

## develop progress v0.02

### 001. todo : I need one init script

* shadowsocket server
* node.js V4 enviroment

### 002. choose nginx as reverse proxy

    adduser nginx
    gpasswd -a nginx sudo
    su - nginx
    sudo apt-get update
    sudo apt-get install nginx
    Ctrl + D

### 003. choose node.js as http server

---
## ~~develop progress v0.0.1 201607192252~~

### 001. prepare domain name cneir.net

### 002. register `cneir` on [Github](https://github.com/remembercode/cneir.git)

### 003. create this `Readme.md`

### 004. write down develop process

### 005. prepare one 5$ vps on digitalocean with `Ubuntu 16.04 x64`

### 006. install nodejs V4

    curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
    sudo apt-get install -y nodejs

### 007. [install Express](https://expressjs.com/en/starter/installing.html)

### 008. [Express HelloWorld](https://expressjs.com/en/starter/hello-world.html)

### 009. for `::ffff:222.131.69.140 - accessed` disable ipv6

### 010. I think I need one-click website




