### CM Install Lab

#### System Configuration Checks

##### 1. Check vm.swappiness on all your nodes o Set the value to 1 if necessary
```
/etc/sysctl.conf 파일에 "vm.swappiness=1" 구문 추가

[centos@ip-172-31-9-97 ~]$ sudo sysctl vm.swappiness=1
vm.swappiness = 1
```

##### 2. Show the mount attributes of your volume(s)
```
[centos@m1: /home/centos]$ df
Filesystem     1K-blocks    Used Available Use% Mounted on
/dev/nvme0n1p1 104846316 1344872 103501444   2% /
devtmpfs         7872836       0   7872836   0% /dev
tmpfs            7895692       0   7895692   0% /dev/shm
tmpfs            7895692   17092   7878600   1% /run
tmpfs            7895692       0   7895692   0% /sys/fs/cgroup
tmpfs            1579140       0   1579140   0% /run/user/1000
```

##### 3. If you have ext-based volumes, list the reserve space setting
```
No ext-based volumes
```

##### 4. Disable transparent hugepage support
```
## /etc/rc.d/rc.local 파일에 아래 구문 추가
1. echo "never" > /sys/kernel/mm/transparent_hugepage/enabled
2. echo "never" > /sys/kernel/mm/transparent_hugepage/defrag

## /etc/default/grub 파일에 아래 구문 추가
1. transparent_hugepage=never (on line GRUB_CMDLINE_LINUX )

## 실행권한 추가
sudo chmod +x /etc/rc.d/rc.local

## disable demon
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
sudo systemctl start tuned
sudo tuned-adm off
sudo tuned-adm list
sudo systemctl stop tuned
sudo systemctl disable tuned
```

##### 5. List your network interface configuration
```
[centos@m1: /home/centos]$ ifconfig
ens5: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 9001
        inet 172.31.9.97  netmask 255.255.240.0  broadcast 172.31.15.255
        inet6 fe80::54:80ff:fe65:7442  prefixlen 64  scopeid 0x20<link>
        ether 02:54:80:65:74:42  txqueuelen 1000  (Ethernet)
        RX packets 931  bytes 360188 (351.7 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 918  bytes 201631 (196.9 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 379  bytes 180665 (176.4 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 379  bytes 180665 (176.4 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```


##### 6. Show that forward and reverse host lookups are correctly resolved
```
#hostname 변경
[centos@m1: ~]$ hostnamectl set-hostname m1.skcc.com
[centos@cm: ~]$ hostnamectl set-hostname cm.skcc.com
[centos@d1: ~]$ hostnamectl set-hostname d1.skcc.com
[centos@d2: ~]$ hostnamectl set-hostname d2.skcc.com
[centos@d3: ~]$ hostnamectl set-hostname d3.skcc.com

#/etc/hosts 파일 수정
15.164.76.42	m1.skcc.com m1
52.78.144.115	cm.skcc.com cm
52.78.158.39	d1.skcc.com d1
52.78.181.163	d2.skcc.com d2
52.78.190.218	d3.skcc.com d3
```

- SSH 키 생성
```
[centos@d3 ~]$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/centos/.ssh/id_rsa):
Enter passphrase (empty for no passphrase): (enter)
Enter same passphrase again: (enter)
Your identification has been saved in /home/centos/.ssh/id_rsa.
Your public key has been saved in /home/centos/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:wouk+1aqVndIYB37eh1y9SfqfyyKELq9cbUkFh7Fzpw centos@d3.com
The key's randomart image is:
+---[RSA 2048]----+
|     ...   ..    |
|    o ..   ..    |
|   . ..   o+..   |
|     ... . +E.   |
|    ..o.S * o o .|
|   o..+=.* = o o |
|  ...++.+ o o  . |
|  ..o  + + o  . o|
| .o+. . o.. oo.o |
+----[SHA256]-----+
```

- SSH 키 복사
```
[centos@d3 .ssh]$ ssh-copy-id centos@m1
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/centos/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
centos@m1's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'centos@m1'"
and check to make sure that only the key(s) you wanted were added.
```

- 각 서버별로 복사 및 접속 확인
```
$ ssh-copy-id centos@m1
$ ssh-copy-id centos@cm
$ ssh-copy-id centos@d1
$ ssh-copy-id centos@d2
$ ssh-copy-id centos@d3
$ ssh m1
$ ssh cm
$ ssh d1
$ ssh d2
$ ssh d3
```

##### 7. Show the nscd service is running
```
# 1. nscd 상태를 체크한다
[centos@ip-172-31-9-97: ~]$ sudo systemctl status nscd
Unit nscd.service could not be found.


2. nscd 서비스를 설치한다

[centos@ip-172-31-9-97: ~]$ sudo yum install -y nscd

3. nscd 서비스를 enable시킨 후에 시작한다

[centos@ip-172-31-9-97: ~]$ sudo systemctl enable nscd
Created symlink from /etc/systemd/system/multi-user.target.wants/nscd.service to /usr/lib/systemd/system/nscd.service.
Created symlink from /etc/systemd/system/sockets.target.wants/nscd.socket to /usr/lib/systemd/system/nscd.socket.
[centos@ip-172-31-9-97: ~]$ sudo systemctl start nscd

4. nscd 상태가 active이면 정상적으로 시작된 것이다.

[centos@ip-172-31-9-97: ~]$ sudo systemctl status nscd
● nscd.service - Name Service Cache Daemon
   Loaded: loaded (/usr/lib/systemd/system/nscd.service; enabled; vendor preset:disabled)
   Active: active (running) since Wed 2019-07-17 02:18:14 UTC; 13s ago
  Process: 21160 ExecStart=/usr/sbin/nscd $NSCD_OPTIONS (code=exited, status=0/SUCCESS)
 Main PID: 21161 (nscd)
   CGroup: /system.slice/nscd.service
           └─21161 /usr/sbin/nscd

Jul 17 02:18:14 ip-172-31-9-97.ap-northeast-2.compute.internal nscd[21161]: 2...
Jul 17 02:18:14 ip-172-31-9-97.ap-northeast-2.compute.internal nscd[21161]: 2...
Jul 17 02:18:14 ip-172-31-9-97.ap-northeast-2.compute.internal nscd[21161]: 2...
Jul 17 02:18:14 ip-172-31-9-97.ap-northeast-2.compute.internal nscd[21161]: 2...
Jul 17 02:18:14 ip-172-31-9-97.ap-northeast-2.compute.internal nscd[21161]: 2...
Jul 17 02:18:14 ip-172-31-9-97.ap-northeast-2.compute.internal nscd[21161]: 2...
Jul 17 02:18:14 ip-172-31-9-97.ap-northeast-2.compute.internal nscd[21161]: 2...
Jul 17 02:18:14 ip-172-31-9-97.ap-northeast-2.compute.internal nscd[21161]: 2...
Jul 17 02:18:14 ip-172-31-9-97.ap-northeast-2.compute.internal nscd[21161]: 2...
Jul 17 02:18:14 ip-172-31-9-97.ap-northeast-2.compute.internal systemd[1]: St...
Hint: Some lines were ellipsized, use -l to show in full.
```

##### 8. Show the ntpd service is running
```
[centos@ip-172-31-9-97 ~]$ sudo yum install ntp
[centos@ip-172-31-9-97 ~]$ sudo service ntpd start
```
