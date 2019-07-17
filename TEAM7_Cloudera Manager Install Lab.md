##### Cloudera Manager Install Lab
#### Path B install using CM 5.15.x
The full rundown is here : https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/5.15.2/


### Step 1: Configure a Repository (5 nodes)
<pre><code>
sudo wget https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/cloudera-manager.repo -P /etc/yum.repos.d/
sudo rpm --import https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/RPM-GPG-KEY-cloudera
</code></pre>

### Step 2: Install JDK (5 nodes)
<pre><code>
[centos@ip-172-31-9-97 ~]$ sudo yum install java-1.8.0-openjdk-devel
</code></pre>

### Step 3: Install Cloudera Manager Server (cm node)
<pre><code>
sudo yum install cloudera-manager-daemons cloudera-manager-server
</code></pre>

### Step 4: Install Cloudera Manager agent: (5 nodes)
<pre><code>
sudo yum install cloudera-manager-daemons cloudera-manager-agent

[centos@m1: /home/centos]$ sudo vi /etc/cloudera-scm-agent/config.ini
[General]
# Hostname of the CM server.
server_host=cm

# Port that the CM server is listening on.
server_port=7182

### Start the Agents by running the following command on all hosts: (cm node를 제외한 4개 node)
sudo systemctl start cloudera-scm-agent
</code></pre>
