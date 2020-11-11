---
layout: post
title: Maven All in One
category: Maven
tags: maven
bg: maven.png
---

# 2020-01-02-maven

Maven使用心得——一个程序小白自学使用maven的心得及体验记录

## maven 安装

1. maven是什么

其实我最初也不知道maven是什么，只知道群里的人聊得挺热闹的，但是我们公司不用maven，用ant编译项目，我连ant也不懂，反正只管写CRUD，改bug，这就是一个刚毕业的程序员进错公司干的事。

不行，我不能这样，不能在这里颓废，但是我又有什么能力跳槽呢？没有经验，家里还负债累累。那就蛰伏吧，自学前卫的技术，慢慢学，翅膀硬了就飞。

1. 安装maven

我使用的是Mac，所以：

```text
vi ~/.bash_profile

export M2_HOME=/Users/alanturing/apache-maven-3.5.0

export PATH=$PATH:$M2_HOME/bin

source ~/.bash_profile
```

搞定。

不出意外的话，使用`mvn -v`就能检查是否安装成功了。

1. maven做什么

跟大多数人一样，最初觉得拿maven导包很不错，也认为maven就这点功能了。不要笑，我真是是个小白。

我以前从来不知道包冲突这回事，知道后来发现怎么也编译不过去。

原来还有包冲突这回事啊，好吧，get到了一个新技能。

```text
<exclusion>是个好东西。
```

1. Maven 中央仓库 Repository

```text
 <mirror>
    <id>nexus-aliyun</id>
    <mirrorOf>*</mirrorOf>
    <name>Nexus aliyun</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```

1. Maven 本地安装

```text
mvn install:install-file -Dfile=E:\sword-lang-2.0.0.jar -DgroupId=sword.org -DartifactId=sword-lang -Dversion=2.0.0 -Dpackaging=jar

mvn install:install-file -Dfile=E:\wechat4j-1.3.1.jar -DgroupId=sword.org -DartifactId=wechat4j -Dversion=1.3.1 -Dpackaging=jar

#如果失败，把每个参数用双引号括起来
mvn install:install-file "-Dfile=D:/wsinforExtract.jar" "-DgroupId=com.cyvation" "-DartifactId=wsinforExtract-092701" "-Dversion=1.0.0" "-Dpackaging=jar"
```

1. maven 编译打包属性到manifest文件中

```markup
<plugin>
    <artifactId>maven-jar-plugin</artifactId>
    <version>3.0.2</version>
    <configuration>
        <archive>
            <manifest>
                <addClasspath>true</addClasspath>
            </manifest>
            <manifestEntries>
                <Main-Class>
                    com.imjcker.App
                </Main-Class>
            </manifestEntries>
        </archive>
    </configuration>
</plugin>
```

1. 指定打包路径

```markup
<configuration>
    <outputDirectory>D:\</outputDirectory>
</configuration>
```

### profile使用

Maven的Profile用于在不同的环境下应用不同的配置。一套配置即称为一个Profile。

Maven提供了Activation机制来激活某个Profile，它既允许自动激活（即在某些条件满足时自动使某个Profile生效），也可以手动激活。

一个Profile几乎可以包含所有能够出现在pom.xml中的配置项，比如，等。相当于在Profile中定义的配置信息会覆盖原有pom.xml中的相应配置项。

#### 定义

## maven plugin 开发

开发自己的maven插件

## maven私有仓库搭建

### 基于nexus

#### 下载nexus包

### 基于GitHub

搭建私有仓库的方法有很多，除了使用nexus外还有一个简便的方法，那就是利用GitHub搭建一个私有仓库。下面以我的maven仓库为例：

1. 在GitHub创建一个repository比如：maven
2. 进入 ${HOME}/.m2/repository/,初始化git本地仓库，添加员段地址。

```text
$ cd ~/.m2/repository
$ git init
$ git remote add origin https://github.com/imjcker/maven.git
```

1. 创建.gitignore 将文件匹配符\*加入其中， 并将.gitignore提交git本地仓库master分支

```bash
$ echo "*" >> .gitignore
$ git add .gitgnore
$ git commit -m 'add .gitignore'
```

1. 分别创建分支snapshot与release并push至远端仓库，用于发布不同状态的artifacts，默认情况切换至snapshot分支

```bash
$ git branch snapshot
$ git branch release
$ git push origin snapshot
$ git push origin release
$ git checkout snapshot
```

1. 当发布新的artifacts时，首先利用mvn install 将artifacts安装至本地maven仓库中 ~/.m2/repository

```bash
$ mvn install
```

然后，将需要发布对应版本的artifacts提交至本地git仓库中，然后push至对应的分支snapshot 或 release.

```bash
$ cd ~/.m2/repository
$ git add -f com/imjcker/jrebel-license-server/1.0.0
$ git commit -m 'snapshot of jrebel-license-server:1.0.0'
$ git push origin snapshot
```

1. 在pom.xml中使用maven artifact添加以下配置：

```markup
<project>
    <repositories>
        <repository>
            <id>imjcker-snapshot-repository</id>
            <name>imjcker-snapshot-repository</name>
            <url>https://raw.github.com/imjcker/maven/snapshot/</url>
        </repository>
        <repository>
            <id>imjcker-release-repository</id>
            <name>imjcker-release-repository</name>
            <url>https://raw.github.com/imjcker/maven/release/</url>
        </repository>
    </repositories>
    <dependencies>
        <dependency>
            <groupId>com.imjcker</groupId>
            <artifactId>jrebel-license-server</artifactId>
            <version>1.0.0</version>
        </dependency>
    </dependencies>
</project>
```

## 上传到私有仓库配置

### 配置nexus

创建2个repository：snapshots、releases。

### 配置maven的settings.xml

配置登录着俩repository的用户名和密码

```markup
    <server>
      <id>snapshots</id>
      <username>admin</username>
      <password>password</password>
    </server>
        <server>
      <id>releases</id>
      <username>admin</username>
      <password>password</password>
    </server>
```

### 配置项目的pom.xml

1. 配置私有仓库地址，并允许发布版和快照版
2. 配置快照和发布版的提交地址

```markup
    <repositories>
        <repository>
            <id>nexus</id>
            <name>Nexus Snapshot Repository</name>
            <url>http://127.0.0.1:8081/repository/maven-public/</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </repository>
    </repositories>

    <distributionManagement>
        <repository>
            <id>releases</id>
            <url>http://localhost:8081/repository/releases</url>
        </repository>
        <snapshotRepository>
            <id>snapshots</id>
            <url>http://localhost:8081/repository/snapshots</url>
        </snapshotRepository>
    </distributionManagement>
```

