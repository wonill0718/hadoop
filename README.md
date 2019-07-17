### hostname 변경
<pre><code>
[centos@ip-172-31-9-97: ~]$ sudo hostname m1
</code></pre>

### /etc/hosts 파일 수정
<pre><code>
15.164.76.42	m1
52.78.144.115	cm
52.78.158.39	d1
52.78.181.163	d2
52.78.190.218	d3
</code></pre>

### private / public key 생성 -> 5개 node에 동일한 private key 배포, authorized_keys 에 5개 노드 등록
<pre><code>
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


### 1. Check vm.swappiness on all your nodes o Set the value to 1 if necessary
<pre><code>
[centos@ip-172-31-9-97 ~]$ sudo sysctl vm.swappiness=1
vm.swappiness = 1
</code></pre>

### 2. Show the mount attributes of your volume(s)

### 3. If you have ext-based volumes, list the reserve space setting o XFS volumes do not support reserve space

### 4. Disable transparent hugepage support

### 5. List your network interface configuration

### 6. Show that forward and reverse host lookups are correctly resolved o For /etc/hosts, use getent o For DNS, use nslookup

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
