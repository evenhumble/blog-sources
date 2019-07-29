---
title: MAVEN In 30 Minutes
date: 2018-06-09 21:34:04
tags:
    - MAVEN
    - CI/CD
    - Project Setup
---

## 1- MAVEN Projects

- use pom.xml to describe the project dependencies,such as third party jars used
- use pom.xml to describe the build process,such as using build plugin to achieve specific building process
- use pom.xml to manage multiple projects/modules, modules are in pom.xml, modules section
- a lot other stuff......

## 2- MAVEN Artifact Vector

MAVEN Project produces an element, JAR,WAR or EAR, uniquely identified by a composite of fields known as groupId,artificatId,packing,version and scope. This vector of fields uniquely distinguishes a MAVEN artifact from all others.

the samples as follow:

```sh
groupid:artifactid:packaging:version:scope
org.springframework:spring:jar:4.3.2:compile
```

## 3- MAVEN Lifecycles

Phases,Plugins and Goals

![img](/images/cicd/maven_lifecycle.jpg)

- Build-in Maven Default Lifecycles:

![img](/images/cicd/maven_default_cl_1.jpg)
![img](/images/cicd/maven_default_cl_2.jpg)

- Site lifecycle

![img](/images/cicd/maven_cl_site.jpg)


## 4- Maven Help

```sh
help: describe -Dplugin=<plugnName>
mvn help:effective-pom
mvn help:active-profiles
```

## 5- MAVEN Dependency

- declaring dependencies

```xml
<dependencies>
    <dependency> <groupId>com.yourcompany</groupId> <artifactId>yourlib</artifactId> <version>1.0</version> <type>jar</type> <scope>compile</scope>
    </dependency>
</dependencies> 
```

- declaring plugins

```xml
 <build>
        <plugins>
                <plugin> <groupId>org.apache.maven.plugins</groupId> <artifactId>maven-compiler-plugin</artifactId> <configuration>
                <maxmem>512m</maxmem> </configuration>
                </plugin> 
        </plugins>
</build>
```

## 6- Maven Scope

scope in dependency:

![img](/images/cicd/maven_scope.jpg)


## 7- Learning Dependencies

To Understand the dependencies for whole project,you can use following commands:

```sh
mvn dependency:tree
mvn dependency:resolve  ## sorted
mvn dependency:resolve-plugins
mvn dependency:analyze
```

## 8- Declaring Repository

Repository is somewhere you can get the jars/wars

```xml
<repositories> 
    <repository>
        <id>JavaDotNetRepo</id>
        <url>https://maven-repository.dev.java.net</url> 
    </repository>
</repositories>
```

## 9- Maven Properties

- Prefefined Properties
![img](/images/cicd/maven_predefined_prop.jpg)

- self defined properties

```xml
<project>
    <properties>
        <my.somevar>My Value</my.somevar>
    </properties>
</project>
```

## 10- Maven Debug

commands for debug:

```sh
mvn -e # stacktrace
mvn -X #
mvnDebug <projectName>
```

## 11- Maven Profile activation

```sh
mvn -P profileName
```

profile setting:

```xml
<profiles> 
    <profile>
    <id>YourProfile</id> [...settings, build, etc...] 
        <activation>
            <os>
            <name>Windows XP</name> <family>Windows</family> <arch>x86</arch> <version>5.1.2600</version>
            </os> <file>
            <missing>somefolder/somefile.txt</missing> </file>
        </activation> 
    </profile>
</profiles>
```

##12- Maven Release

```sh
mvn release: repare
mvn release: perform
```

## 13- Maven Reports

reports defined in reporting section, these reports could be coverage report, and any other reports.
