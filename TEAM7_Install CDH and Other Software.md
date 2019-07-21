### CM Install Lab
#### Install CDH and Other Software

##### 1. Choose which edition to install
--> Cloudera Enterprise Cloudera Enterprise Trial, which does not require a license, but expires after 60 days and cannot be renewed.

##### 2. Specify hosts for your CDH cluster installation
hostname, IP 등을 활용하여 클러스트를 구성할 노드를 선택한다.
<img src="https://github.com/gichul-hong/hadoop-test-2019/blob/master/image/2_host_list.PNG" />

##### 3. Select Repository
<img src="https://github.com/gichul-hong/hadoop-test-2019/blob/master/image/3_version.PNG" />

##### 4. JDK 설치
서버를 구성하면서 open jdk 1.8 버전을 설치했으므로, 여기서는 skip

##### 5. Single User Mode
사용하지 않는다.

##### 6. Enter Login Credentials
centos  계정 및 password  입력

##### 7. Install Agents / Parcels / Inspect hosts for correctness
-->Install Agents
<img src="https://github.com/gichul-hong/hadoop-test-2019/blob/master/image/4_install_agent.PNG" />

--> Install Clusters
<img src="https://github.com/gichul-hong/hadoop-test-2019/blob/master/image/5_install_cluster.PNG" />


##### 8. Select Services
1. Custom Services : Yarn, HDFS, zookeeper 선택 후 설치
2. 필요한 경우 추후 add service 를 통해 설치

##### 9. Assign Roles
1. namenode, resource manager 등은 master node에, 기타 유틸성 데몬은 cm 서버 쪽에 셋업한다.
<img src="https://github.com/gichul-hong/hadoop-test-2019/blob/master/image/6_cluster_role_setting.PNG" />

### 10. Setup Database
<img src="https://github.com/gichul-hong/hadoop-test-2019/blob/master/image/7_DB_setting.PNG" />

### 11. First Run Command
<img src="https://github.com/wonill0718/hadoop/blob/master/image/cloudera manager.png"></img>

### 12. Add Service - oozie, hive, hue
<img src="https://github.com/wonill0718/hadoop/blob/master/image/hive.png"></img>
<img src="https://github.com/wonill0718/hadoop/blob/master/image/hive2.png"></img>
<img src="https://github.com/wonill0718/hadoop/blob/master/image/hue.png"></img>
<img src="https://github.com/wonill0718/hadoop/blob/master/image/oozie.png"></img>
