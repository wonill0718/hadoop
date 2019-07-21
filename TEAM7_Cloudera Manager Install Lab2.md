### Cloudera Manager Install Lab

##### Step 1: Start the cms
```
## 데몬 구동
sudo systemctl start cloudera-scm-server

## log 확인
sudo tail -f /var/log/cloudera-scm-server/cloudera-scm-server.log

#When you see this log entry, the Cloudera Manager Admin Console is ready:
INFO WebServerImpl:com.cloudera.server.cmf.WebServerImpl: Started Jetty server.
```

### Step 2: connect to cms
--> http://<server_host>:7180   (admin : admin)
