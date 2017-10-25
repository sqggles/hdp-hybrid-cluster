### Setup Steps for HDP/HDP 2.6 Hybrid Cluster with Kylo/Nifi, Dremio, and Superset/Druid

#### Initial HDP 2.6 Cluster Setup with Cloudbreak
[Cloudbreak Service](https://10.247.25.90/sl/?logout=true&source=aHR0cDovLzEwLjI0Ny4yNS45MA==)
[Ambari UI](https://10.247.25.118/ambari/#/login)

1. Created HDP 2.6 cluster by editing existing minimal HDP 2.5 cluster template in Cloudbreak and updating the stack version to 2.6.
2. It creates the following cluster nodes
![Alt text](./Screenshot 2017-10-25 09.37.50.png)
3. With the following services running 
![Alt text](./Screenshot 2017-10-25 09.39.08.png)



#### Adding HDF Services to Running HDP Cluster

Added Ranger, Druid, Registry, Atlas, Kafka, Superset, Nifi, Streaming Analytics Manager after installing HDF Management Pack per instructions here: https://docs.hortonworks.com/HDPDocuments/HDF3/HDF-3.0.1.1/bk_installing-hdf-and-hdp/content/ch_install-mpack.html

Used Ambari UI to allocate master and worker nodes across various services
Used MySQL db for Ranger data store, setting up ambari server as per [Ranger on HDP Installation](#Ranger_On_HDP) instructions below:

#### Ranger on HDP

1. You must have an MySQL/Oracle/Postgres/MSSQL/SQL Anywhere Server database instance running to be used by Ranger.
1. In Assign Masters step of this wizard, you will be prompted to specify which host for the Ranger Admin. On that host, you must have DB Client installed for Ranger to access to the database. (Note: This is applicable for only Ranger 0.4.0)
1. Ensure that the access for the DB Admin user is enabled in DB server from any host.
1. Execute the following command on the Ambari Server host. Replace database-type with `mysql|oracle|postgres|mssql|sqlanywhere` and `/jdbc/driver/path` based on the location of corresponding JDBC driver:
```
ambari-server setup --jdbc-db={database-type} --jdbc-driver={/jdbc/driver/path}

ambari-server setup --jdbc-db=mysql --jdbc-driver=/opt/jdbc-drivers/mysql-connector-java-5.1.39-bin.jar
```

#### NiFi Install Failure and Resolution

Encountered error in NiFi installation :
```
File “/var/lib/ambari-agent/cache/common-services/NIFI/1.0.0/package/scripts/params.py”, line 47, in <module> stack_version_buildnum = get_component_version_with_stack_selector(“/usr/bin/hdf-select”, “nifi”) NameError: name ‘get_component_version_with_stack_selector’ is not defined

```
Seems similar to: http://margus.roo.ee/2017/07/03/ambari-2-5-0-3-install-hdf-nifi/. So edited the following file on each node as per instructions above.
``` 
sudo vim /var/lib/ambari-agent/cache/common-services/NIFI/1.0.0/package/scripts/params.py
```
