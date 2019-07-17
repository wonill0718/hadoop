##### MySQL Installation Lab
#### MySQL installation - Plan Two Detail

### Step 1. MySQL Installation(cm node)
<pre><code>
#1. wget install
sudo yum install wget

#2. my-sql install
cd ~
sudo wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm
sudo yum update
sudo yum install mysql-server
sudo systemctl start mysqld

#3. my-sql execute
mysql
</code></pre>

### Step 2: Install and Configure Databases(cm node)
<pre><code>
#1 mysql jdbc driver 다운 / 업로드 / 카피
[centos@cm: /home/centos]$ sudo wget http://central.maven.org/maven2/mysql/mysql-connector-java/5.1.47/mysql-connector-java-5.1.47.jar
[centos@cm: /home/centos]$ sudo mkdir -p /usr/share/java/
[centos@cm: /home/centos]$ cp mysql-connector-java-5.1.47.jar /usr/share/java/
[centos@cm: /home/centos]$ sudo cp mysql-connector-java-5.1.47.jar /usr/share/java/mysql-connector-java.jar

[centos@cm: /home/centos]$ sudo scp /usr/share/java/mysql-connector-java.jar centos@m1:/usr/share/java/mysql-connector-java.jar

[centos@cm: /home/centos]$ sudo scp /usr/share/java/mysql-connector-java.jar centos@d1:/usr/share/java/mysql-connector-java.jar

[centos@cm: /home/centos]$ sudo scp /usr/share/java/mysql-connector-java.jar centos@d2:/usr/share/java/mysql-connector-java.jar

[centos@cm: /home/centos]$ sudo scp /usr/share/java/mysql-connector-java.jar centos@d3:/usr/share/java/mysql-connector-java.jar

#2 Creating Databases for Cloudera Software  
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


# 3 Move old InnoDB log files to backup location
<pre><code>
sudo mv /var/lib/mysql/ib_logfile0 ~/
sudo mv /var/lib/mysql/ib_logfile1 ~/
</code></pre>

# 4 Update my.conf  
<pre><code>
sudo vi /etc/my.cnf

[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
transaction-isolation = READ-COMMITTED
# Disabling symbolic-links is recommended to prevent assorted security risks;
# to do so, uncomment this line:
symbolic-links = 0

key_buffer_size = 32M
max_allowed_packet = 32M
thread_stack = 256K
thread_cache_size = 64
query_cache_limit = 8M
query_cache_size = 64M
query_cache_type = 1

max_connections = 550
#expire_logs_days = 10
#max_binlog_size = 100M

#log_bin should be on a disk with enough free space.
#Replace '/var/lib/mysql/mysql_binary_log' with an appropriate path for your
#system and chown the specified folder to the mysql user.
log_bin=/var/lib/mysql/mysql_binary_log

#In later versions of MySQL, if you enable the binary log and do not set
#a server_id, MySQL will not start. The server_id must be unique within
#the replicating group.
server_id=1

binlog_format = mixed

read_buffer_size = 2M
read_rnd_buffer_size = 16M
sort_buffer_size = 8M
join_buffer_size = 8M

# InnoDB settings
innodb_file_per_table = 1
innodb_flush_log_at_trx_commit  = 2
innodb_log_buffer_size = 64M
innodb_buffer_pool_size = 4G
innodb_thread_concurrency = 8
innodb_flush_method = O_DIRECT
innodb_log_file_size = 512M

[mysqld_safe]
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid

sql_mode=STRICT_ALL_TABLES
</code></pre>


# 5 Enable MySQL server starts at boot / Strat the MySQL server
<pre><code>
sudo systemctl enable mysqld
sudo systemctl start mysqld
</code></pre>
