# Hadoop ver 2 Cluster Configuration (master1 one NN + DN),(slave1 one DN)+(slave2 one DN)

- ### In windows vmnet1 configure the 4.4.4.10

- ### Open vmware on linux (master1) login with root

- ### Configure the IP-address permanently

```
then click on network icon ---->vpn connection ---->configure vpn ----> wired ----connect automatically---->select eth0---->edit ----> give the ip address 4.4.4.100 and subnetmask 255.0.0.0 ----> apply ---->close

rc on desktop ----> open terminal
```

```
service network restart
```

```
ifconfig
```

```
ping 4.4.4.10 (windows machine)
```

- ### Stop the firewall in Linux

```
service iptables stop
chkconfig iptables off
```

- ### Now open the putty , login with root and do your work

- ### Add a user who is superuser of hadoop

```
useradd hadoop
passwd hadoop
```

- ### Install and configure java
  > winscp the java software(jdk-7u65-linux-i586_2.rpm) into root

```
rpm -i jdk-7u65-linux-i586_2.rpm

java -version
```

- ### If java is installed then uninstall it.

```
rpm -qa|grep -i java
```

```
rpm -e --nodeps java-1.6.0-openjdk-1.6.0.0-1.45.1.11.1.el6.i686
```

```
rpm -e --nodeps java-1.5.0-gcj-1.5.0.0-29.1.el6.i686
```

- ### If java version shows incorrect then check all the latest jdk version---

```
rpm -qa |egrep -i "(jdk|jre)"
rpm -qa |egrep "(jdk)"

rpm -e --nodeps ("latest version of ur jdk")
```

- ### Then again unpack the jdk -

```
rpm -i jdk-7u65-linux-i586_2.rpm

java -version
```

- ### Configure /etc/hosts

- ### Add the lines

```
nano /etc/hosts

4.4.4.100 master1
4.4.4.101 slave1
4.4.4.102 slave2
4.4.4.10 windows
```

#### save it

```
cat /etc/hosts
```

- ### Shutdown master1

```
init 0
```

- ### copy master1 as slave1

- ### Open slave1 to change the hostname and IP-address

- ### Login with root

- ### Configure the IP-address permanently

```

then click on network icon ---->vpn connection ---->configure vpn ----> wired ----connect automatically ----delete eth0---->select eth1---->edit ----> give the ip address 4.4.4.2 and subnetmask 255.0.0.0 ----> apply ---->close
```

- ### Change the hostname

```
nano /etc/sysconfig/network

change master1 to slave1
```

- ### Reboot your slave1

```
init 6
```

- ### Start master1 and login with putty by hadoop user in master1

## Handshaking between master1 and slave1

- ### Generating the public key

```
ssh-keygen -t rsa -P ""

cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

chmod 600 ~/.ssh/authorized_keys
```

- ### Copy the public key of master1 to slave1 from master1

```
scp -r ~/.ssh slave1:~/

enter the pasword hadoop

ssh slave1 ----From master we can direct login to slave without password

exit
```

> winscp hadoop v2 into master1 by hadoop user

```
ls

tar -xzf hadoop-2.4.1.tar.gz

mv hadoop-2.4.1 hadoop
```

- ### Exporting the hadoop conf path

```

export HADOOP_HOME=/home/hadoop/hadoop
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export JAVA_HOME=/usr/java/jdk1.7.0_65
export PATH=$PATH:$HADOOP_HOME/sbin:\$HADOOP_HOME/bin

```

- ### Save the path permanently in .bash_profile file

```

cd

nano .bash_profile
export HADOOP_HOME=/home/hadoop/hadoop
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export JAVA_HOME=/usr/java/jdk1.7.0_65
export PATH=$PATH:$HADOOP_HOME/sbin:\$HADOOP_HOME/bin

```

#### save the file

- ### Test hadoop is installed

```

hadoop version

```

- ### Write the hadoop conf in core-site.xml

```

cd \$HADOOP_HOME/etc/hadoop

```

```xml
nano core-site.xml

<configuration>
<property>
  <name>fs.default.name</name>
      <value>hdfs://master1:9000</value>
      </property>
</configuration>
```

#### save the file

```xml
nano hdfs-site.xml

<configuration>
<property>
 <name>dfs.replication</name>
 <value>2</value>
</property>
<property>
  <name>dfs.name.dir</name>
    <value>file:///home/hadoop/hadoopdata/hdfs/namenode</value>
</property>
<property>
  <name>dfs.data.dir</name>
    <value>file:///home/hadoop/hadoopdata/hdfs/datanode</value>
</property>
</configuration>
```

#### save the file

```xml
nano mapred-site.xml

<configuration>
 <property>
  <name>mapreduce.framework.name</name>
   <value>yarn</value>
 </property>
</configuration>
```

#### save the file

```xml
nano yarn-site.xml

<configuration>
 <property>
  <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
 </property>
</configuration>
```

#### save the file

```
nano \$HADOOP_HOME/etc/hadoop/hadoop-env.export

sh JAVA_HOME=/usr/java/jdk1.7.0_65
```

#### save the file

```
nano slaves
delete localhost
master1
slave1
slave2
```

#### save the file

- ### Copy the hadoop dir from master1 to slave1

```
cd

scp -r hadoop slave1:/home/hadoop
scp .bash_profile slave1:/home/hadoop

scp -r hadoop slave2:/home/hadoop
scp .bash_profile slave2:/home/hadoop
```

- ### Now goto slave1

```
ssh slave1
cd \$HADOOP_HOME/etc/hadoop
nano slaves
delete the master1,slave2 line
```

#### save the file

#### exit

- ### Now goto slave2

```
ssh slave2
cd \$HADOOP_HOME/etc/hadoop
nano slaves
delete the master1,slave1 line
```

#### save the file

#### exit

- ### From master1 format the hdfs

```
hdfs namenode -format

start-all.sh

slaves.sh /usr/java/jdk1.7.0_65/bin/jps

hadoop dfsadmin -report|more

hadoop fs -ls /

hadoop fs -mkdir /srm
```

> if the error is ** No Route to Host from slave1/1.1.1.26 to master1:9000 failed on socket timeout exception: java.net.NoRouteToHostException: No route to host; For more details see: http://wiki.apache.org/hadoop/NoRouteToHost **

> then from root user of namenode.....

```
/sbin/iptables -L -n

/etc/init.d/iptables save

/etc/init.d/iptables stop

/sbin/iptables -L -n
```
