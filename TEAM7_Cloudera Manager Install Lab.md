#### Path B install using CM 5.15.x
The full rundown is here : https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/5.15.2/


### Step 1: Configure a Repository (5nodes)
<pre><code>
sudo wget https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/cloudera-manager.repo -P /etc/yum.repos.d/
sudo rpm --import https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/RPM-GPG-KEY-cloudera
</code></pre>

### Step 2: Install JDK (5nodes)
<pre><code>
[centos@ip-172-31-9-97 ~]$ sudo yum install java-1.8.0-openjdk-devel
</code></pre>

### Step 3: Install Cloudera Manager Server (cm서버에서)
<pre><code>
sudo yum install cloudera-manager-daemons cloudera-manager-server

#### On all hosts, run the following command to install the Cloudera Manager agent: (5nodes)
sudo yum install cloudera-manager-daemons cloudera-manager-agent

### On all hosts, configure the Cloudera Manager Agent to point to the Cloudera Manager Server by setting the following properties in the /etc/cloudera-scm-agent/config.ini configuration file:
<pre><code>
[centos@m1: /home/centos]$ sudo vi /etc/cloudera-scm-agent/config.ini

[General]
# Hostname of the CM server.
server_host=cm

# Port that the CM server is listening on.
server_port=7182
</code></pre>

### Start the Agents by running the following command on all hosts: (cm을 제외한 4개 node)
sudo systemctl start cloudera-scm-agent
</code></pre>


### Step 4: Install and Configure Databases
<pre><code>
페이지 참조


#### Ensure the MySQL server starts at boot:



### mysql jdbc driver 다운 / 업로드 / 카피 (cm에서)
[centos@m1: /home/centos]$ sudo mkdir -p /usr/share/java/
[centos@m1: /home/centos]$ cp mysql-connector-java-5.1.47.jar /usr/share/java/
[centos@m1: /home/centos]$ sudo cp mysql-connector-java-5.1.47.jar /usr/share/java/


#### Creating Databases for Cloudera Software


CREATE DATABASE cmserver DEFAULT CHARACTER SET utf8;
GRANT ALL on cmserver.* TO 'cmserveruser'@'%' IDENTIFIED BY 'skcc';

CREATE DATABASE metastore DEFAULT CHARACTER SET utf8;
GRANT ALL on metastore.* TO 'hiveuser'@'%' IDENTIFIED BY 'skcc';

CREATE DATABASE amon DEFAULT CHARACTER SET utf8;
GRANT ALL on amon.* TO 'amonuser'@'%' IDENTIFIED BY 'skcc';

CREATE DATABASE rman DEFAULT CHARACTER SET utf8;
GRANT ALL on rman.* TO 'rmanuser'@'%' IDENTIFIED BY 'skcc';

CREATE DATABASE oozie DEFAULT CHARACTER SET utf8;
GRANT ALL on oozie.* TO 'oozieuser'@'%' IDENTIFIED BY 'skcc';

CREATE DATABASE hue DEFAULT CHARACTER SET utf8;
GRANT ALL on hue.* TO 'hueuser'@'%' IDENTIFIED BY 'skcc';
</code></pre>


### Step 5: Set up the Cloudera Manager Database
