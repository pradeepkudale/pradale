---
layout: post
title:  "Spring Boot Packaging"
date:   2020-11-02
categories: spring-boot
tags: spring-boot spring-boot-maven-plugin
author: pradale
description: The spring boot maven plugin helps us to create archives (jar files and war files) which contains all the application dependencies.  
---
The spring-boot-maven-plugin can be used to create archives (jar or war files) which contains all the application dependencies.

The default packaging in maven is **jar**
>>> <packaging>war</packaging>

We can add the plugin in 'plugins' section of pom.xml. But if we use [spring-boot-starter-parent](/spring-boot-starter-parent) project to manage our dependencies 
then we don't need to add the plugin in pom.xml file.

### Dependency Exclusion from Packing
Dependencies can be excluded from packing. There are two ways one can exclude a dependency from being packaged.

* Exclude the artifact by groupId and artifactId.

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <excludes>                    
                    <exclude>
                        <!-- if you want to exclude specific dependency -->
                        <groupId>com.hazelcast</groupId>
                        <artifactId>hazelcast</artifactId>
                    </exclude>                    
                </excludes>
            </configuration>
        </plugin>
    </plugins>
</build>
```
* Exclude the artifact by just groupId.

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <!-- If you want to exclude all the dependencies which belong to com.hazelcast groupId. -->
                <excludeGroupIds>com.hazelcast</excludeGroupIds>
            </configuration>
        </plugin>
    </plugins>
</build>
```

### Plugin Goals
The spring-boot-maven-plugin has below important goals.

{:class="table table-bordered"}
|---
 Goals        | Usage           
 ------------- |-------------
 spring-boot:run  | : Run the application
 spring-boot:start | : Start the application. Used in integration test.
 spring-boot:stop | : Stops the application which has been started by start goal.

Sample execution
> mvn spring-boot:run

### Layered Jar  
Your packages jar will have three types of entries and will look something like this:

```
example.jar
 |
 +-META-INF
 |  +-MANIFEST.MF
 +-org
 |  +-springframework
 |     +-boot
 |        +-loader
 |           +-<spring boot loader classes>
 +-BOOT-INF
    +-classes
    |  +-mycompany
    |     +-project
    |        +-YourClasses.class
    +-lib
       +-dependency1.jar
       +-dependency2.jar
```

```
example.war
 |
 +-META-INF
 |  +-MANIFEST.MF
 +-org
 |  +-springframework
 |     +-boot
 |        +-loader
 |           +-<spring boot loader classes>
 +-WEB-INF
    +-classes
    |  +-com
    |     +-mycompany
    |        +-project
    |           +-YourClasses.class
    +-lib
    |  +-dependency1.jar
    |  +-dependency2.jar
    +-lib-provided
       +-servlet-api.jar
       +-dependency3.jar
```

* **BOOT-INF/classes** contains the application classes.
* **BOOT-INF/lib** contains the application dependencies.
* **BOOT-INF/classpath.idx** file provides the list of jar file names in the order that they should be added to classpath.
* **org/springframework/boot/loader/*.class** is responsible to support the executable jar and war files.


### Summary
In this tutorial, we saw how we can use spring-boot-maven-plugin plugin for packing.

Source code for this tutorial is available on [Github](https://github.com/pradeepkudale/pradale-tutorials/tree/main/spring-boot/spring-boot-starter-parent)
