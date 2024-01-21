# Hadoop

## 大数据概论

海量数据的处理

### 分布式处理

|              | 传统分布式计算         | Hadoop分布式计算     |
| ------------ | ---------------------- | -------------------- |
| 计算方式     | 将数据复制到计算节点   | 在不同节点并行计算   |
| 可处理数据量 | 小数据量               | 大数据两             |
| CPU限制      | 受CPU限制大            | 受单台设备限制少     |
| 计算能力     | 提升单台计算机计算能力 | 扩展低成本服务器集群 |





## Hadoop架构

Hadoop起源于Apache Nutch，是一个开源分布式系统架构，解决海量数据存储计算的问题，陈伟海量数据处理的架构首选。

非常快的完成大数据计算任务



架构包括

HDFS分布式文件系统

MapReduce分布式计算框架

YARN分布式资源管理系统

Common支持其他模块的公共工具程序





网技实践

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240121110925962.png" alt="image-20240121110925962" style="zoom:80%;" />



Hadoop安装

```shell
tar -zxf hadoop-2.6.0-cdh5.14.2.tar.gz
mv hadoop-2.6.0-cdh5.14.2 soft/hadoop260
vim /etc/profile
cd soft/hadoop260/etc/hadoop



```

外部存储hdfs

```
         <property>
                <name></name>
                <value></value>
        <\property>
```



```xml
修改hadoop-env.sh:
export JAVA_HOME=/opt/soft/jdk1.8.0

修改core-site.xml:
<configuration>
        <property>
                <name>fs.defaultFS</name>
                <value>hdfs://192.168.158.143:9000</value>
        </property>
        <property>
                <name>hadoop.tmp.dir</name>
                <value>/opt/soft/hadoop260/tmp</value>
        </property>

        <property>
                <name>hadoop.proxyuser.root.hosts</name>
                <value>*</value>
        </property>
        <property>
                <name>hadoop.proxyuser.root.groups</name>
                <value>*</value>
        </property>
      <property>
                <name>hadoop.proxyuser.root.users</name>
                <value>*</value>
        </property>
</configuration>


修改hdfs-site.xml:
<configuration>
        <property>
                <name>dfs.replication</name>
                <value>1</value>
        <\property>
</configuration>
```

配置计算引擎

```
cp mapred-site.xml.template mapred-site.xml
vim mapred-site.xml
```

```
<property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
<\property>

```

配置yarn调度

```
vim yarn-site.xml
```

```
<property>
        <name>yarn.resourcemanager.localhost</name>
        <value>localhost</value>
<\property>
<property>
       <name>yarn.nodemanager.aux-services</name>
       <value>mapreduce_shuffle</value>
<\property>
```

```
vim /etc/profile


export HADOOP_HOME=/opt/soft/hadoop260 
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
export HADOOP_INSTALL=$HADOOP_HOME
```

无密登录  

```
ssh-keygen -t rsa -P ''
```

```
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): 
/root/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:o/HK8vvD0HIh+Yb4Yj5GGB2MJh1rp8iKRtmdXS5YR1s root@localhost.localdomain
The key's randomart image is:
+---[RSA 2048]----+
| ..+     . E     |
|. +.o   . o      |
| oo... o +       |
|.o+oo B =        |
|.+.+ =.BSo       |
|+ . o ++*.       |
|o. . ..*.        |
|.   *...o        |
|   +.==o..       |
+----[SHA256]-----+

```

![image-20240121155456191](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240121155456191.png)



检查密钥

```
cat id_rsa
cat id_rsa.pub
```

![image-20240121155512983](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240121155512983.png)



发送密钥

```shell
ssh-copy-id -i /root/.ssh/id_rsa.pub root@192.168.158.145
```

```shell
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/root/.ssh/id_rsa.pub"
The authenticity of host '192.168.158.145 (192.168.158.145)' can't be established.
ECDSA key fingerprint is SHA256:uzdQr0QZfptlDqhm+Pc09t2t6HSvpwzXKaNN7+Qez+U.
ECDSA key fingerprint is MD5:97:3b:21:c8:0c:9d:fa:03:9b:fd:d4:9a:79:97:03:27.
Are you sure you want to continue connecting (yes/no)? yes//连接其他计算机
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@192.168.158.145's password: //输入密码

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'root@192.168.158.145'"
and check to make sure that only the key(s) you wanted were added.

```

登录另外的机子效果

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240121160227147.png" alt="image-20240121160227147" style="zoom:80%;" />

格式化

```
 hdfs namenode -format
```

启动

```
start-all.sh
```





双绞线正线和反线

可以提供交换

 