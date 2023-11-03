# Install Hadoop in Windows 7
A guid to walk through the Hadoop installation process on a Windows 7 (for Windows 10 see [How to install Hadoop in 5 Steps in Windows 10](https://medium.com/analytics-vidhya/hadoop-how-to-install-in-5-steps-in-windows-10-61b0e67342f8)). 
This tutorial will show you how to install Hadoop on Windows 7, breaking down the installation into clear steps. 

## Table of Contents

- [I- Download Files](#)
- [II- Setup Folders and Files](#)
- [III- Setup Environment Variables](#)
- [IV- Setup Configuration Files](#)
- [V- Testing](#)
- [VI- Some Hadoop Commands](#)
- [VII- Stop Hadoop](#)

## I- Download Files 

* Download Hadoop [hadoop-2.9.2.tar.gz](https://archive.apache.org/dist/hadoop/core/hadoop-2.9.2/)
![hadoop-2.9.2](./screens/I-Download_Files/1-hadoop-2.9.2.png)

* Download [java 11](https://www.oracle.com/java/technologies/javase/jdk11-archive-downloads.html)
![java11](./screens/I-Download_Files/2-java11.png)

* Download [winutils](https://github.com/cdarlint/winutils/tree/master), we will need the bin files that are in [hadoop-2.9.2/bin](https://github.com/cdarlint/winutils/tree/master/hadoop-2.9.2/bin)
![winutils](./screens/I-Download_Files/3-winutils.png)

## II- Setup Folders and Files 

1. **Create new folder in `C:\` named `hadoop`** 
We will exptact `hadoop-2.9.2.tar.gz` to this folder `C:\hadoop\`.

2. **Created 3 folders** 
- First folder named `data`, should be created in `C:\hadoop\hadoop-2.9.2\`. Like `C:\hadoop\hadoop-2.9.2\data`.
- Second folder named `datanode`, should be created in `C:\hadoop\hadoop-2.9.2\data\`. Like `C:\hadoop\hadoop-2.9.2\data\datanode`.
- Third folder named `namenode`, should be created also in `C:\hadoop\hadoop-2.9.2\data\`. Like `C:\hadoop\hadoop-2.9.2\data\namenode`.

3. **Extract the `winutils-master.zip` file** 
We are using Hadoop 2.9.2, so we will copy all files that are in the bin folder of hadoop-2.9.2 folder `winutils-master\hadoop-2.9.2\bin\`, to `%HADOOP_HOME%\bin`. Replacing all files.


## III- Setup Environment Variables 
click on `windows key` then search for `environment variables`, then click on `edit environment variables for your account`
![win_key](./screens/III-Setup_Environment_Variables/win_key.PNG)

1. **JAVA_HOME :** 
If you don't have a `JAVA_HOME` variable, click on `new` to add it :
![click_new_envirenment_variable](./screens/III-Setup_Environment_Variables/click_new_envirenment_variable.PNG)
- Then in the `variable name` type : 
```plaintext 
JAVA_HOME
```
- And in the `variable value` type : 
```plaintext 
C:\java\jdk-11.0.19
```
![JAVA_HOME_envirenment_variable](./screens/III-Setup_Environment_Variables/JAVA_HOME_envirenment_variable.PNG)

2. **HADOOP_HOME :** 
- Click on `new` to add `HADOOP_HOME` variable, then in the `variable name` type : 
```plaintext 
HADOOP_HOME
```
- And in the `variable value` type : 
```plaintext 
C:\hadoop\hadoop-2.9.2
```
![HADOOP_HOME_envirenment_variable](./screens/III-Setup_Environment_Variables/HADOOP_HOME_envirenment_variable.PNG)


3. **Path :** 
Scroll to `Path` and select it, then click on `Edit`, and add this to the begining of  `variable value` type : 
```plaintext 
%JAVA_HOME%\bin;%HADOOP_HOME%\bin;%HADOOP_HOME%\sbin;
```
![Path_envirenment_variable](./screens/III-Setup_Environment_Variables/Path_envirenment_variable.PNG)


4. **Verifying :** 
* Verifying `Java` : 
Open cmd and type :
```batch
java -version
```
The output should be like : 
```plaintext 
java version "11.0.19" 2023-04-18 LTS
Java(TM) SE Runtime Environment 18.9 (build 11.0.19+9-LTS-224)
Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.19+9-LTS-224, mixed mode)
```
![verifying_1_java-version](./screens/III-Setup_Environment_Variables/verifying_1_java-version.PNG)


* Verifying `JAVA_HOME` :
In cmd type :
```batch
echo %JAVA_HOME%
```
The output should be like : 
```plaintext 
C:\java\jdk-11.0.19
```
![verifying_2_JAVA_HOME](./screens/III-Setup_Environment_Variables/verifying_2_JAVA_HOME.PNG)


* Verifying `HADOOP_HOME` :
In cmd type :
```batch
echo %HADOOP_HOME%
```
The output should be like : 
```plaintext 
C:\hadoop\hadoop-2.9.2
```
![verifying_3_HADOOP_HOME](./screens/III-Setup_Environment_Variables/verifying_3_HADOOP_HOME.PNG)


* Verifying `PATH` :
In cmd type :
```batch
echo %PATH%
```
The output should be like : 
```plaintext 
C:\java\jdk-11.0.19\bin;C:\hadoop\hadoop-2.9.2\bin;C:\hadoop\hadoop-2.9.2\sbin;C:\Use...
```
![verifying_4_PATH](./screens/III-Setup_Environment_Variables/verifying_4_PATH.PNG)

## IV- Setup Configuration Files 

Go to `C:\hadoop\hadoop-2.9.2\etc\hadoop` to find the file that we will edit.

1. **Modifying the `core-site.xml` file :** 
```plaintext
<configuration>
    <property>		
        <name>hadoop.tmp.dir</name>
        <value>C:\hadoop\hadoop-2.9.2\tmp</value>
    </property>

    <property>
        <name>fs.default.name</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

2. **Modifying the `hdfs-site.xml` file :** 
```plaintext
<configuration>
  <property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>

  <property>
    <name>dfs.namenode.name.dir</name>
    <value>C:\hadoop\hadoop-2.9.2\data\namenode</value>
  </property>

  <property>
    <name>dfs.datanode.data.dir</name>
    <value>C:\hadoop\hadoop-2.9.2\data\datanode</value>
  </property>
</configuration>
```

3. **Modifying the `mapred-site.xml` file :** 
```plaintext
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```

4. **Modifying the `yarn-site.xml` file :** 
```plaintext
<configuration>
  <property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
  </property>

  <property>
    <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
    <value>org.apache.hadoop.mapred.ShuffleHandler</value>
  </property>
</configuration>
```

5. **Verifying the `hadoop-env.cmd` file :** 
- Make sure that JAVA_HOME is set correctly :
```plaintext
    set JAVA_HOME=%JAVA_HOME%
```



## V- Testing 

## VI- Some Hadoop Commands 

## VII- Stop Hadoop 

