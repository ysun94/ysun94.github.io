---
layout: "post"
date: 2018-09-26
title: How to Install Hadoop and Spark on macOS Mojave
---


I am interested high-frequency trading involving big data processing based on the orderflows. For better analysis, the Hadoop is required. Thus I have tried to install Hadoop and Spark on my MacBook, and decided to write down the process of the installation as my first blog.

# Install Hadoop
## Step 1: Install **HomeBrew**
You can also refer to the [official website](https://brew.sh) for the installation.

If you are not sure whether the HomeBrew has been installed, please check by running following command:

```bash
$ brew -v
```
which will show the version of the HomeBrew installed.

If the HomeBrew was not installed, run

```bash
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## Step 2: Install Java
Apache Hadoop is an open source platform built on Linux operating system and Java programming language. Thus, before installing Hadoop, we need to install Java environment.

Go to the [official website](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) and install **Java SE Development Kit**. For macOS users, select Mac OS X x64 version to install (don't forget to accept licence agreement).

![Java Installation](http://ysun94.github.io/assets/images/20180925HadoopJavaInstall.png)

Then install it based on the instruction.

Edit ``~/.bash_profile``

```bash
if which java > /dev/null; then export JAVA_HOME=$(/usr/libexec/java_home); fi
Install Apache Spark
```

## Step 3: Install **Hadoop**
It is easy to install Hadoop with HomeBrew by pasting the following command in the terminal prompt:

```bash
$ brew install hadoop
```

## Step 4: Configure Hadoop
Hadoop is installed in the directory ``/usr/local/Cellar/hadoop``. Now, change your current directory into ``/usr/local/Cellar/hadoop/3.1.1/libexec/etc/hadoop/``, where ``3.1.1`` is the version of the Hadoop installed.

### 1. Configure ``hadoop-env.sh``
Then open ``hadoop-env.sh``.

If you did not installed any editor on your computer, just open it by the built-in TextEditor. Try

```bash
$ open -a TextEditor hadoop-env.sh
```
I prefer using VS code, so I just simply run

```bash
$ code hadoop-env.sh
```

change

```sh
export HADOOP_OPTS="$HADOOP_OPTS -Djava.net.preferIPv4Stack=true"
```
to

```sh
export HADOOP_OPTS="$HADOOP_OPTS -Djava.net.preferIPv4Stack=true -Djava.security.krb5.realm= -Djava.security.krb5.kdc="
```

Also, add

```sh
export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk1.8.0_181.jdk/Contents/Home"
```

### 2. ``core-site.xml``
Then configure HDFS (Hadoop Distributed File System) address and port number, open ``core-site.xml``, input following content in ``<configuration></configuration>`` tag.

```bash
code core-site.xml
```
Paste the content at a proper area in your editor.

```xml
<configuration>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/usr/local/Cellar/hadoop/hdfs/tmp</value>
        <description>A base for other temporary directories.</description>
    </property>
    <property>
        <name>fs.default.name</name>
        <value>hdfs://localhost:8020</value>
    </property>
</configuration>
```

### 3. ``mapred-site.xml``

Configure ``jobtracker`` address and port number in map-reduce in the similar way. Open ``mapred-site.xml`` and add

```xml
<configuration>
    <property>
        <name>mapred.job.tracker</name>
        <value>localhost:8021</value>
    </property>
</configuration>
```

### 4. ``hdfs-site.xml``

Open ``hdfs-site.xml`` and edit it as well.

```xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```


## Step 5. Format HDFS
Before running background program, we should format the installed HDFS first. Run the command

```bash
hdfs namenode -format
```

Then the termial shows a long information like:

```bash
2018-09-26 12:01:45,842 INFO namenode.NameNode: STARTUP_MSG: 
/************************************************************
STARTUP_MSG: Starting NameNode
STARTUP_MSG:   host = s-MacBook-puro.local/192.168.0.100
STARTUP_MSG:   args = [-format]
STARTUP_MSG:   version = 3.1.1
.....
2018-09-26 12:01:51,511 INFO namenode.NameNode: SHUTDOWN_MSG: 
/************************************************************
SHUTDOWN_MSG: Shutting down NameNode at s-MacBook-puro.local/192.168.0.100
************************************************************/
```

It means that we finish HDFS configuration, and Hadoop is ready to launch.

## Step 6: Enable SSH

Check if ~/.ssh/id_rsa and the~/.ssh/id_rsa.pub files exist. If so, move forward, or execute the following command in Terminal:

```bash
$ ssh-keygen -t rsa
```

Enable Remote Login by navigating the following path :“System Preferences” -> “Sharing”. Check “Remote Login”. 

Now you have to Authorize SSH Keys to make the system aware of the keys that will be used so that it accepts login. You can do this by using this:

```bash
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

Test ssh at localhost:

```bash
$ ssh localhost
```
If success, you will see some informarion like:

```bash
s-MacBook-puro:hadoop sunyan$ ssh localhost
Last login: Wed Sep 26 11:26:17 2018
```

## Step 7: Alias to start and stop Hadoop Daemons


Edit ```~/.bash_profile``` and add

``` bash
alias hstart="/usr/local/Cellar/hadoop/2.8.2/sbin/start-all.sh"
alias hstop="/usr/local/Cellar/hadoop/2.8.2/sbin/stop-all.sh"
```

Then run

```bash
source ~/.bash_profile
```

## Step 8: Run Hadoop
Start hadoop with

```bash
hstart
```
Then go to http://localhost:9870, and you will see the following page:

![Hadoop Overview](http://ysun94.github.io/assets/images/20180926HadoopOverview.png)


# Install Apache Spark

```bash
brew install scala
brew install apache-spark
```

## Set up env variables

Add following code to your ``.bash_profile``

```bash
# For a ipython notebook and pyspark integration
if which pyspark > /dev/null; then
  export SPARK_HOME="/usr/local/Cellar/apache-spark/2.1.0/libexec/"
  export PYTHONPATH=$SPARK_HOME/python:$SPARK_HOME/python/build:$PYTHONPATH
  export PYTHONPATH=$SPARK_HOME/python/lib/py4j-0.10.4-src.zip:$PYTHONPATH
fi
```

You can check ``SPARK_HOME`` path using following brew command

```
$ brew info apache-spark
apache-spark: stable 2.3.1, HEAD
Engine for large-scale data processing
https://spark.apache.org/
/usr/local/Cellar/apache-spark/2.3.1 (1,018 files, 243.8MB) *
  Built from source on 2018-09-20 at 22:35:01
From: https://github.com/Homebrew/homebrew-core/blob/master/Formula/apache-spark.rb
==> Requirements
Required: java = 1.8 ✔
==> Options
--HEAD
	Install HEAD version
==> Analytics
install: 3,881 (30d), 15,426 (90d), 53,420 (365d)
install_on_request: 3,758 (30d), 13,934 (90d), 47,177 (365d)
build_error: 0 (30d)
```
## Run IPython

```bash
$ jupyter-notebook
Initialize pyspark
```

```python
In [1]: import os
        execfile(os.path.join(os.environ["SPARK_HOME"], 'python/pyspark/shell.py'))
Out[1]: <pyspark.context.SparkContext at 0x10a982b10>
```


sc variable should be available

```python
In [2]: sc
Out[2]: <pyspark.context.SparkContext at 0x10a982b10>
```

## Alternatively

You can also force ``pyspark`` shell command to run ipython web notebook instead of command line interactive interpreter. To do so you have to add following env variables:

```bash
export PYSPARK_DRIVER_PYTHON=jupyter
export PYSPARK_DRIVER_PYTHON_OPTS=notebook
```

then simply run

```bash
$ pyspark
```

which will open a web notebook with sc available automatically.