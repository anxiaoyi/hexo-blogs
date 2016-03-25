---
title: "Install JDK on ubuntu manually"
layout: post
date: 2015-11-28
tags: 
- jdk
- install
categories:
- java
---

[How Can I install sun oracles proprietary java jdk](http://askubuntu.com/questions/56104/how-can-i-install-sun-oracles-proprietary-java-jdk-6-7-8-or-jre)

- Download
- `tar -xzvf jdk-8-linux-x64.tar.gz`
- Now move the JDK 8 directory to `/usr/lib`
```bash
sudo mkdir -p /usr/lib/jvm
sudo mv ./jdk1.8.0 /usr/lib/jvm/
```

- Now run 
```bash
sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk1.8.0/bin/java" 1
sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk1.8.0/bin/javac" 1
sudo update-alternatives --install "/usr/bin/javaws" "javaws" "/usr/lib/jvm/jdk1.8.0/bin/javaws" 1
```

This will assign Oracle JDK a priority of 1.