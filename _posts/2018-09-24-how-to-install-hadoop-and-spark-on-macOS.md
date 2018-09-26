---
layout: "post"
date: 2018-09-25
title: How to Install Hadoop and Spark on macOS Mojave
---


I am interested high-frequency trading involving big data processing based on the orderflows. For better analysis, the Hadoop is required. Thus I have tried to install Hadoop and Spark on my MacBook, and decided to write down the process of the installation as my first blog.

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

```bash
export HADOOP_OPTS="$HADOOP_OPTS -Djava.net.preferIPv4Stack=true"
```
to

```bash
export HADOOP_OPTS="$HADOOP_OPTS -Djava.net.preferIPv4Stack=true -Djava.security.krb5.realm= -Djava.security.krb5.kdc="
```

Also, add

```bash
export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk1.8.0_181.jdk/Contents/Home"
```