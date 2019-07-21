### Cloudera Manager Install Lab

#### Step 1: Configure a Repository (at cm node)
```
## .repo 파일을 받아온 후, 설치할 버전에 맞게 baseurl 을 수정
sudo wget https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/cloudera-manager.repo -P /etc/yum.repos.d/
sudo rpm --import https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/RPM-GPG-KEY-cloudera

sudo vi /etc/yum.repos.d/cloudera-manager.repo 
baseurl=https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/5.15.4/
```

#### Step 2: Install JDK (5 nodes)
```
[centos@ip-172-31-9-97 ~]$ sudo yum install java-1.8.0-openjdk-devel
```

#### Step 3: Install Cloudera Manager Server (at cm node)
```
sudo yum install cloudera-manager-daemons cloudera-manager-server
```

#### Step 4: Install Cloudera Manager agent (나중에 GUI 를 통해서도 설치 가능함..)
```
sudo yum install cloudera-manager-daemons cloudera-manager-agent

[centos@m1: /home/centos]$ sudo vi /etc/cloudera-scm-agent/config.ini
[General]
# Hostname of the CM server.
server_host=cm.skcc.com

# Port that the CM server is listening on.
server_port=7182

### Start the Agents by running the following command on all hosts: (cm node를 제외한 4개 node)
sudo systemctl start cloudera-scm-agent
```
