#### System Configuration Checks

### 1. Check vm.swappiness on all your nodes o Set the value to 1 if necessary
<pre><code>
[centos@ip-172-31-9-97 ~]$ sudo sysctl vm.swappiness=1
vm.swappiness = 1
</code></pre>

### 2. Show the mount attributes of your volume(s)
<pre><code>
[centos@m1: /home/centos]$ df
Filesystem     1K-blocks    Used Available Use% Mounted on
/dev/nvme0n1p1 104846316 1344872 103501444   2% /
devtmpfs         7872836       0   7872836   0% /dev
tmpfs            7895692       0   7895692   0% /dev/shm
tmpfs            7895692   17092   7878600   1% /run
tmpfs            7895692       0   7895692   0% /sys/fs/cgroup
tmpfs            1579140       0   1579140   0% /run/user/1000
</code></pre>

### 3. If you have ext-based volumes, list the reserve space setting
<pre><code>
Not exist ext-based volumes
</code></pre>

### 4. Disable transparent hugepage support
<pre><code>
# cat /sys/kernel/mm/transparent_hugepage/enabled
always madvise [never]
</code></pre>

### 5. List your network interface configuration
<pre><code>
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
</code></pre>

### 6. Show that forward and reverse host lookups are correctly resolved
<pre><code>
#hostname 변경
[centos@ip-172-31-9-97: ~]$ sudo hostname m1

#/etc/hosts 파일 수정
15.164.76.42	m1
52.78.144.115	cm
52.78.158.39	d1
52.78.181.163	d2
52.78.190.218	d3

#private / public key 생성 -> 5개 node에 동일한 private key 배포, authorized_keys 에 5개 노드 등록
[centos@d3 .ssh]$ ls -al
total 16
drwx------. 2 centos centos   71 Jul 17 03:44 .
drwx------. 3 centos centos  104 Jul 17 03:46 ..
-rw-rw-r--. 1 centos centos 2528 Jul 17 02:23 1
-rw-------. 1 centos centos 2306 Jul 17 03:44 authorized_keys
-rw-------. 1 centos centos 1676 Jul 17 02:06 id_rsa
-rw-r--r--. 1 centos centos 1043 Jul 17 02:24 known_hosts
[centos@d3 .ssh]$ cat authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCJ48Vo3WQEHM/P2Fv8Nby9eFOKM/DR84Bc7bZIT5ZAKdOuHEq8WgallS2b01grR1tcQ0qJG9UCqDV0gBOhGSUj9AwqjRTNlhijSgSDSmoAqVLgJQViT4LW1NMQwRFeHABlqmGVMCZcB8muriwhIyCyCHJM/gPOFltI4VtV0z6emoofacpody/i4vpNEdMwniMN/ZUttaSdejiA4ShA36KsgrFJgGVNYJzLUfkptAdTI59D+cg4jasISzxAh4sSeMf4LQvcMkK0/VCIUEKcYzFKzyyCLRW3I0b6vFqdXVy4ob3q9/XW5Yv0xLtJ1CyQF0PbhGvOly0VUDJV3RcRVrSv skcc
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDN5mynD0n8wrbkrRfNfnSxLzXBfisd8pESLRCEFMfE2bXaO8O4+/4d3sHPJfiUcGaNxkS2rhgBWBYzu+oU/XiavURrgoMnRxrpJKNanuDP+4Vwq1ISKuKe7nkgpgOrk3M7hKGn51x6wLOUBSc04PHEc6Sj6JxuqcnO8tfHHaSuVLNwGaBKeCTgVrl8PJaSdG7vDxnr1uzCwUbtpE+lEvl9D++nLxdYBs5LEKMaqQN33Ogl5/nuWN37pFXJEpR24l/zYFV/k+dURkg3FUZS8PxC9DFvHQ84yuScoWDbKSpty9NHSuiEjbvSkei+aKIxhLiLCtISkTm8U4JEvbanB1/7 m1
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDN5mynD0n8wrbkrRfNfnSxLzXBfisd8pESLRCEFMfE2bXaO8O4+/4d3sHPJfiUcGaNxkS2rhgBWBYzu+oU/XiavURrgoMnRxrpJKNanuDP+4Vwq1ISKuKe7nkgpgOrk3M7hKGn51x6wLOUBSc04PHEc6Sj6JxuqcnO8tfHHaSuVLNwGaBKeCTgVrl8PJaSdG7vDxnr1uzCwUbtpE+lEvl9D++nLxdYBs5LEKMaqQN33Ogl5/nuWN37pFXJEpR24l/zYFV/k+dURkg3FUZS8PxC9DFvHQ84yuScoWDbKSpty9NHSuiEjbvSkei+aKIxhLiLCtISkTm8U4JEvbanB1/7 cm
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDN5mynD0n8wrbkrRfNfnSxLzXBfisd8pESLRCEFMfE2bXaO8O4+/4d3sHPJfiUcGaNxkS2rhgBWBYzu+oU/XiavURrgoMnRxrpJKNanuDP+4Vwq1ISKuKe7nkgpgOrk3M7hKGn51x6wLOUBSc04PHEc6Sj6JxuqcnO8tfHHaSuVLNwGaBKeCTgVrl8PJaSdG7vDxnr1uzCwUbtpE+lEvl9D++nLxdYBs5LEKMaqQN33Ogl5/nuWN37pFXJEpR24l/zYFV/k+dURkg3FUZS8PxC9DFvHQ84yuScoWDbKSpty9NHSuiEjbvSkei+aKIxhLiLCtISkTm8U4JEvbanB1/7 d1
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDN5mynD0n8wrbkrRfNfnSxLzXBfisd8pESLRCEFMfE2bXaO8O4+/4d3sHPJfiUcGaNxkS2rhgBWBYzu+oU/XiavURrgoMnRxrpJKNanuDP+4Vwq1ISKuKe7nkgpgOrk3M7hKGn51x6wLOUBSc04PHEc6Sj6JxuqcnO8tfHHaSuVLNwGaBKeCTgVrl8PJaSdG7vDxnr1uzCwUbtpE+lEvl9D++nLxdYBs5LEKMaqQN33Ogl5/nuWN37pFXJEpR24l/zYFV/k+dURkg3FUZS8PxC9DFvHQ84yuScoWDbKSpty9NHSuiEjbvSkei+aKIxhLiLCtISkTm8U4JEvbanB1/7 d2
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDN5mynD0n8wrbkrRfNfnSxLzXBfisd8pESLRCEFMfE2bXaO8O4+/4d3sHPJfiUcGaNxkS2rhgBWBYzu+oU/XiavURrgoMnRxrpJKNanuDP+4Vwq1ISKuKe7nkgpgOrk3M7hKGn51x6wLOUBSc04PHEc6Sj6JxuqcnO8tfHHaSuVLNwGaBKeCTgVrl8PJaSdG7vDxnr1uzCwUbtpE+lEvl9D++nLxdYBs5LEKMaqQN33Ogl5/nuWN37pFXJEpR24l/zYFV/k+dURkg3FUZS8PxC9DFvHQ84yuScoWDbKSpty9NHSuiEjbvSkei+aKIxhLiLCtISkTm8U4JEvbanB1/7 d3
</code></pre>

### 7. Show the nscd service is running
<pre><code>
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
   Loaded: loaded (/usr/lib/systemd/system/nscd.service; enabled; vendor preset:                                    disabled)
   Active: active (running) since Wed 2019-07-17 02:18:14 UTC; 13s ago
  Process: 21160 ExecStart=/usr/sbin/nscd $NSCD_OPTIONS (code=exited, status=0/S                                   UCCESS)
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
</pre></code>

### 8. Show the ntpd service is running
<pre><code>
[centos@ip-172-31-9-97 ~]$ sudo yum install ntp
[centos@ip-172-31-9-97 ~]$ sudo service ntpd start
</code></pre>
