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

## Step 2: Install **Hadoop**
It is easy to install Hadoop with HomeBrew by pasting the following command in the terminal prompt:

```bash
$ brew install hadoop
```

## Step 3: Configure Hadoop
Hadoop is installed in the directory ``/usr/local/Cellar/hadoop``. Now, change your current directory into ``/usr/local/Cellar/hadoop/3.1.1/libexec/etc/hadoop/``, where ``3.1.1`` is the version of the Hadoop installed.

Then open ``hadoop-env.sh``.

If you did not installed any editor on your computer, just open it by the built-in TextEditor. Try

```bash
$ open -a TextEditor hadoop-env.sh
```
I prefer using VS code, so I just simply run

```bash
$ code hadoop-env.sh
```