# SH Cluster Configuration

#### Set up the Deployer ```(splnk-cmng)```

vi /opt/splunk/etc/system/local/server.conf

```
[shclustering]
pass4SymmKey = <secret>
shcluster_label = <sh_label>
```

/opt/splunk/bin/splunk restart

#### Initialize the Instance

#### Run on each member of the SH cluster (splnk-sh1, splnk-sh2, splnk-sh3)
```
/opt/splunk/bin/splunk init shcluster-config -mgmt_uri https://splnk-sh1:8089 -conf_deploy_fetch_url https://splnk-cmng:8089 -replication_port 9200 -replication_factor 2 -secret <secret> -shcluster_label <sh_label> -auth admin:pa$$w0rd
```

/opt/splunk/bin/splunk init shcluster-config -mgmt_uri https://splnk-sh2:8089 -conf_deploy_fetch_url https://splnk-cmng:8089 -replication_port 9200 -replication_factor 2 -secret <secret> -shcluster_label <sh_label> -auth admin:pa$$w0rd  
  
 
/opt/splunk/bin/splunk init shcluster-config -mgmt_uri https://splnk-sh3:8089 -conf_deploy_fetch_url https://splnk-cmng:8089 -replication_port 9200 -replication_factor 2 -secret <secret> -shcluster_label <sh_label> -auth admin:pa$$w0rd
  
/opt/splunk/bin/splunk restart
  
#### Configure Captain (splnk-sh1)
/opt/splunk/bin/splunk bootstrap shcluster-captain -servers_list "https://splnk-sh1:8089, https://splnk-sh2:8089, https://splnk-sh3:8089" -auth admin:pa$$w0rd
  
  
