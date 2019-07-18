##### CM Install Lab
#### Install CDH and Other Software

### 1. Choose which edition to install
--> Cloudera Enterprise Cloudera Enterprise Trial, which does not require a license, but expires after 60 days and cannot be renewed.

### 2. Specify hosts for your CDH cluster installation
<img src="https://github.com/wonill0718/hadoop/blob/master/image/host-search.PNG"></img>

### 3. Select Repository
<img src="https://github.com/wonill0718/hadoop/blob/master/image/cluster-installation.PNG"></img>

### 4. JDK ->  기설치함, skip

### 5. Single User Mode --> 사용안함

### 6. Enter Login Credentials
--> centos  계정 및 password  입력

### 7. Install Agents / Parcels / Inspect hosts for correctness

### 8. Select Services
--> Custom Services : Yarn, HDFS, zookeeper 선택 후 설치

### 9. Assign Roles


### 10. Setup Database
--> 데몬이 사용할 데이터베이스 및 계정정보 입력 ->  Test max_connections

### 11. First Run Command
<img src="https://github.com/wonill0718/hadoop/blob/master/image/cloudera manager.png"></img>

### 12. Add Service - oozie, hive, hue
<img src="https://github.com/wonill0718/hadoop/blob/master/image/hive.png"></img>
<img src="https://github.com/wonill0718/hadoop/blob/master/image/hive2.png"></img>

<img src="https://github.com/wonill0718/hadoop/blob/master/image/hue.png"></img>

<img src="https://github.com/wonill0718/hadoop/blob/master/image/oozie.png"></img>
