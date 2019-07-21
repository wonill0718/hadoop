### MySQL Installation Lab

#### Step 1. MySQL Installation(at cm node)
```
#1. wget install
sudo yum install -y wget

#2. my-sql install
sudo wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm
sudo yum update
sudo yum install mysql-server
sudo systemctl start mysqld

#3. my-sql execute
mysql -uroot
```

#### Step 2: Install and Configure Databases(at cm node)
```
#1 mysql jdbc driver 다운 / 업로드 / 카피
[centos@cm: /home/centos]$ sudo wget http://central.maven.org/maven2/mysql/mysql-connector-java/5.1.47/mysql-connector-java-5.1.47.jar
[centos@cm: /home/centos]$ sudo mkdir -p /usr/share/java/
[centos@cm: /home/centos]$ cp mysql-connector-java-5.1.47.jar /usr/share/java/mysql-connector-java.jar

# 타 노드에 MYSQL JDBC 드라이버 복사
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
```

#### Step 5:  Enable MySQL server starts at boot / Strat the MySQL server
```
sudo systemctl enable mysqld
sudo systemctl start mysqld
```


#### Step 6: Preparing the Cloudera Manager Server Database
```
[centos@cm: /home/centos]$ sudo /usr/share/cmf/schema/scm_prepare_database.sh mysql cmserver cmserveruser skcc
```
